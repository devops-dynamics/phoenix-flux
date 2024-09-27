# 3. Implementation (3 weeks)

## 3.1 Infrastructure Setup

### Terraform for OCI Resource Provisioning

1. Set up Terraform configuration:

   Create a `main.tf` file with the following structure:

   ```hcl
   provider "oci" {
     tenancy_ocid     = var.tenancy_ocid
     user_ocid        = var.user_ocid
     fingerprint      = var.fingerprint
     private_key_path = var.private_key_path
     region           = var.region
   }

   resource "oci_core_vcn" "vcn" {
     cidr_block     = "10.0.0.0/16"
     compartment_id = var.compartment_id
     display_name   = "project-vcn"
   }

   # Add subnets, internet gateway, route tables, and security lists here

   resource "oci_core_instance" "k8s_node" {
     count               = 2
     availability_domain = data.oci_identity_availability_domain.ad.name
     compartment_id      = var.compartment_id
     shape               = "VM.Standard.E2.1.Micro"
     # Add more configuration for the instances
   }

   # Add OKE cluster resource
   ```

2. Create necessary variables in a `variables.tf` file.

3. Initialize Terraform and apply the configuration:
   ```
   terraform init
   terraform plan
   terraform apply
   ```

### Set up Kubernetes Cluster using OKE

1. Use Terraform to create an OKE cluster:

   Add to `main.tf`:
   ```hcl
   resource "oci_containerengine_cluster" "k8s_cluster" {
     compartment_id     = var.compartment_id
     kubernetes_version = var.kubernetes_version
     name               = "project-cluster"
     vcn_id             = oci_core_vcn.vcn.id
     # Add more configuration
   }

   resource "oci_containerengine_node_pool" "k8s_node_pool" {
     cluster_id         = oci_containerengine_cluster.k8s_cluster.id
     compartment_id     = var.compartment_id
     kubernetes_version = var.kubernetes_version
     name               = "project-node-pool"
     node_shape         = "VM.Standard.E2.1.Micro"
     # Add more configuration
   }
   ```

2. After applying Terraform, configure kubectl:
   ```
   oci ce cluster create-kubeconfig --cluster-id <cluster_id> --file $HOME/.kube/config --region <region> --token-version 2.0.0
   ```

3. Verify cluster setup:
   ```
   kubectl get nodes
   ```

## 3.2 Application Development

### Develop a Simple Web Application (Python Flask)

1. Create a new directory for your application:
   ```
   mkdir flask-app && cd flask-app
   ```

2. Create a `requirements.txt` file:
   ```
   Flask==2.0.1
   prometheus-flask-exporter==0.18.2
   ```

3. Create `app.py`:
   ```python
   from flask import Flask, jsonify
   from prometheus_flask_exporter import PrometheusMetrics

   app = Flask(__name__)
   metrics = PrometheusMetrics(app)

   @app.route('/')
   def hello():
       return jsonify({"message": "Hello, World!"})

   @app.route('/healthz')
   def health():
       return jsonify({"status": "healthy"}), 200

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

### Containerize the Application using Docker

1. Create a `Dockerfile`:
   ```dockerfile
   FROM python:3.9-slim

   WORKDIR /app

   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt

   COPY app.py .

   EXPOSE 5000

   CMD ["python", "app.py"]
   ```

2. Build and test the Docker image:
   ```
   docker build -t flask-app:v1 .
   docker run -p 5000:5000 flask-app:v1
   ```

3. Push the image to a container registry (e.g., Oracle Cloud Container Registry):
   ```
   docker tag flask-app:v1 <region>.ocir.io/<tenancy-namespace>/flask-app:v1
   docker push <region>.ocir.io/<tenancy-namespace>/flask-app:v1
   ```

## 3.3 Monitoring and Self-Healing Implementation

### Deploy Prometheus for Monitoring

1. Create a `prometheus-values.yaml` file for Helm:
   ```yaml
   server:
     persistentVolume:
       enabled: false
   alertmanager:
     persistentVolume:
       enabled: false
   ```

2. Install Prometheus using Helm:
   ```
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm install prometheus prometheus-community/prometheus -f prometheus-values.yaml
   ```

### Implement Health Checks and Readiness Probes

1. Create a Kubernetes deployment YAML (`deployment.yaml`):
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: flask-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: flask-app
     template:
       metadata:
         labels:
           app: flask-app
       spec:
         containers:
         - name: flask-app
           image: <region>.ocir.io/<tenancy-namespace>/flask-app:v1
           ports:
           - containerPort: 5000
           livenessProbe:
             httpGet:
               path: /healthz
               port: 5000
             initialDelaySeconds: 10
             periodSeconds: 5
           readinessProbe:
             httpGet:
               path: /healthz
               port: 5000
             initialDelaySeconds: 5
             periodSeconds: 5
   ```

2. Apply the deployment:
   ```
   kubectl apply -f deployment.yaml
   ```

## 3.4 Auto-scaling Setup

### Implement Horizontal Pod Autoscaler (HPA)

1. Create an HPA YAML (`hpa.yaml`):
   ```yaml
   apiVersion: autoscaling/v2beta1
   kind: HorizontalPodAutoscaler
   metadata:
     name: flask-app-hpa
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: flask-app
     minReplicas: 3
     maxReplicas: 10
     metrics:
     - type: Resource
       resource:
         name: cpu
         targetAverageUtilization: 50
   ```

2. Apply the HPA:
   ```
   kubectl apply -f hpa.yaml
   ```

3. Verify HPA setup:
   ```
   kubectl get hpa
   ```

## Deliverables

### GitHub Repository

1. Create a new GitHub repository for your project.

2. Structure your repository:
   ```
   /
   ├── terraform/
   │   ├── main.tf
   │   ├── variables.tf
   │   └── outputs.tf
   ├── kubernetes/
   │   ├── deployment.yaml
   │   └── hpa.yaml
   ├── flask-app/
   │   ├── app.py
   │   ├── requirements.txt
   │   └── Dockerfile
   ├── prometheus/
   │   └── prometheus-values.yaml
   └── README.md
   ```

3. Commit and push your code to GitHub:
   ```
   git add .
   git commit -m "Initial project setup"
   git push origin main
   ```

### Deployed Application in OCI

1. Verify your application is running:
   ```
   kubectl get pods
   kubectl get services
   ```

2. Set up an Ingress controller or LoadBalancer service to expose your application.

### Configured Monitoring and Auto-scaling

1. Verify Prometheus is running:
   ```
   kubectl get pods -n prometheus
   ```

2. Set up Grafana (optional) for visualization:
   ```
   helm install grafana grafana/grafana
   ```

3. Verify HPA is working:
   ```
   kubectl get hpa
   ```

4. Test auto-scaling by generating load on your application.