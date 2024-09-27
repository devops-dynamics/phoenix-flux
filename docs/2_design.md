# 2. Design (1 week)

## 2.1 Architecture Design

### Three-Tier Application Architecture

We'll design a simple three-tier application consisting of:

1. Web Tier: Nginx (Reverse Proxy)
   - Handles incoming HTTP requests
   - Load balances requests to the application servers
   - Serves static content

2. Application Tier: Python Flask
   - Processes business logic
   - Interacts with the database
   - Handles dynamic content generation

3. Database Tier: PostgreSQL
   - Stores and manages application data

This architecture will be containerized and deployed on Kubernetes for better scalability and easier management.

#### Kubernetes Architecture:

- Kubernetes Cluster (using OKE - Oracle Container Engine for Kubernetes)
  - Master Node (managed by OKE)
  - Worker Nodes (2 nodes using OCI's Always Free tier)
    - Each node will run:
      - Nginx pods
      - Flask application pods
      - PostgreSQL pod (single instance for simplicity)
      - Prometheus pod
      - Node Exporter pods (for hardware and OS metrics)

### Monitoring Setup with Prometheus

1. Prometheus Server:
   - Deployed as a pod in the Kubernetes cluster
   - Configured to scrape metrics from:
     - Kubernetes API server
     - Nodes (via Node Exporter)
     - Application pods (via Flask exporter)

2. Node Exporter:
   - Deployed as a DaemonSet to ensure it runs on all nodes
   - Collects system-level metrics (CPU, memory, disk, network)

3. Flask Exporter:
   - Integrated into the Flask application
   - Exposes application-specific metrics (request count, response times, etc.)

4. Alertmanager:
   - Handles alerts sent by Prometheus server
   - Configured to send notifications (e.g., to a webhook or email)

5. Grafana (Optional, if time permits):
   - Provides a dashboard for visualizing Prometheus metrics

### Self-Healing Scenarios Design

1. Container Restart:
   - Kubernetes Liveness Probe:
     - Configured for Flask pods
     - Performs HTTP GET request to /healthz endpoint
     - If probe fails consecutively, Kubernetes restarts the container

2. Node Failure Recovery:
   - Kubernetes ReplicaSet:
     - Ensures desired number of pod replicas are running
     - If a node fails, Kubernetes reschedules pods to healthy nodes

3. Application Error Recovery:
   - Kubernetes Readiness Probe:
     - Configured for Flask pods
     - Checks if the application is ready to serve traffic
     - If probe fails, Kubernetes removes pod from service endpoint

4. Resource-based Scaling:
   - Horizontal Pod Autoscaler (HPA):
     - Configured to monitor CPU utilization of Flask pods
     - Automatically scales the number of pods based on CPU usage

## 2.2 Tool Selection and Setup

### GitHub Setup

1. Create a GitHub account (if not already done)
2. Create a new repository for the project
3. Set up project board for task management
4. Configure branch protection rules for main branch

### Oracle Cloud Infrastructure (OCI) Setup

1. Sign up for an Oracle Cloud account
2. Set up OCI CLI for local development
3. Create a new compartment for the project
4. Generate necessary API keys and configure local environment

### Local Development Environment Setup

1. Docker:
   - Install Docker Desktop (Windows/Mac) or Docker Engine (Linux)
   - Verify installation: `docker --version`

2. kubectl:
   - Install kubectl using package manager or direct download
   - Verify installation: `kubectl version --client`

3. Terraform:
   - Download and install Terraform
   - Verify installation: `terraform version`

4. Oracle Cloud Infrastructure CLI:
   - Install OCI CLI using script or package manager
   - Configure OCI CLI: `oci setup config`

5. Python and pip:
   - Install Python 3.8+ and pip
   - Verify installations: `python --version` and `pip --version`

6. Visual Studio Code (or preferred IDE):
   - Install VS Code
   - Install relevant extensions:
     - Python
     - Docker
     - Kubernetes
     - HashiCorp Terraform

### Additional Tools (Optional, based on time constraints)

1. Helm:
   - Package manager for Kubernetes
   - Simplifies deployment of applications

2. Grafana:
   - For creating dashboards to visualize Prometheus metrics

3. Postman:
   - For testing API endpoints

## Deliverables

### 1. Architecture Diagram

Create a comprehensive architecture diagram using a tool like draw.io or Lucidchart. The diagram should include:

- Overall system architecture
- Kubernetes cluster layout
- Network flow
- Monitoring setup
- Self-healing components

### 2. Tool Setup Guide

Create a detailed guide for setting up the development environment. Include:

1. Prerequisites
2. Step-by-step instructions for each tool installation
3. Configuration steps for OCI and GitHub
4. Verification steps to ensure correct setup
5. Troubleshooting tips for common issues

### 3. Design Document

Create a design document that includes:

1. Detailed description of the three-tier application
2. Kubernetes deployment strategy
3. Monitoring architecture and metrics to be collected
4. Self-healing scenarios and their implementation
5. Security considerations
6. Scaling strategy