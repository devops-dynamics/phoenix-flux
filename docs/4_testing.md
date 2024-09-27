# 4. Testing (1 week)

## 4.1 Functionality Testing

### Application Deployment and Basic Functionality

1. Deployment Verification:
   - Command: `kubectl get deployments`
   - Expected: Deployment status should be "Available"

2. Pod Status Check:
   - Command: `kubectl get pods`
   - Expected: All pods should be in "Running" state

3. Service Accessibility:
   - Command: `kubectl get services`
   - Action: Note the external IP/port of your service

4. Basic Functionality Test:
   - Tool: curl or Postman
   - Action: Send GET request to your application's endpoints
   - Expected: Receive appropriate responses (e.g., HTTP 200 OK)

   Example:
   ```bash
   curl http://<external-ip>:<port>/
   Expected: {"message": "Hello, World!"}

   curl http://<external-ip>:<port>/healthz
   Expected: {"status": "healthy"}
   ```

5. Logging Verification:
   - Command: `kubectl logs <pod-name>`
   - Expected: Should see application logs, including incoming requests

### Monitoring Metric Verification

1. Prometheus Target Check:
   - Access: Prometheus UI (typically at `http://<prometheus-service-ip>:9090`)
   - Navigate to: Status -> Targets
   - Expected: Your application endpoints should be listed and "Up"

2. Metric Exploration:
   - In Prometheus UI, go to Graph
   - Query examples:
     - `flask_http_request_total`: Total HTTP requests
     - `flask_http_request_duration_seconds`: Request duration
   - Expected: Should see data for these metrics

3. Custom Metric Verification:
   - If you've added custom metrics, query them in Prometheus
   - Example: `custom_metric_name`
   - Expected: Should see data if the metric is correctly implemented

4. Grafana Dashboard (if implemented):
   - Access Grafana UI
   - Check pre-configured dashboards for your application metrics
   - Expected: Dashboards should display real-time data

## 4.2 Self-Healing Scenario Testing

### Simulate Container Crashes and Verify Automatic Restarts

1. Intentional Pod Termination:
   - Command: `kubectl delete pod <pod-name>`
   - Expected: Kubernetes should automatically create a new pod

2. Verify Pod Recreation:
   - Command: `kubectl get pods`
   - Expected: A new pod should appear with a different ID, status should become "Running"

3. Liveness Probe Failure Simulation:
   - Modify your application to fail the `/healthz` endpoint temporarily
   - Push the updated image and redeploy
   - Expected: Kubernetes should restart the pod after probe failures

4. Readiness Probe Test:
   - Modify application to temporarily fail readiness checks
   - Observe in Kubernetes dashboard or with `kubectl get pods`
   - Expected: Pod should not receive traffic until readiness check passes

5. Node Failure Simulation:
   - If possible, simulate a node failure in your OCI environment
   - Expected: Pods should be rescheduled to other available nodes

### Auto-scaling Test Using Apache JMeter

1. JMeter Setup:
   - Download and install Apache JMeter
   - Create a basic test plan for your application

2. Basic Load Test Configuration:
   - Thread Group: Start with 50 users, ramp-up over 30 seconds
   - HTTP Request: Point to your application's endpoint
   - Add listeners: View Results Tree, Aggregate Report

3. Execute Basic Load Test:
   - Run the JMeter test plan
   - Monitor application response in JMeter listeners

4. Observe Auto-scaling:
   - Command: `kubectl get hpa`
   - Expected: Should see HPA increasing the number of pods

5. Increasing Load Test:
   - Gradually increase the number of users in JMeter
   - Observe how HPA responds by scaling up pods

6. Cool-down Test:
   - After load test, observe how system scales down
   - Command: `kubectl get pods`
   - Expected: Number of pods should decrease after load reduces

7. Performance Metrics Collection:
   - During load tests, collect metrics from Prometheus/Grafana
   - Metrics to focus on:
     - Response time
     - Error rate
     - CPU and Memory usage
     - Number of pods

8. Stress Test:
   - Configure JMeter for maximum expected load
   - Observe system behavior under stress
   - Verify if auto-scaling handles the load as expected

## Deliverable: Test Results Document

Create a comprehensive test results document including:

1. Executive Summary:
   - Brief overview of testing performed
   - Key findings and results

2. Test Environment:
   - Description of OCI setup
   - Kubernetes cluster details
   - Application version tested

3. Functionality Test Results:
   - Deployment verification results
   - Basic functionality test outcomes
   - Screenshots of successful API responses

4. Monitoring Test Results:
   - Prometheus target status
   - Screenshots of key metrics in Prometheus/Grafana
   - Any issues or anomalies observed

5. Self-Healing Test Results:
   - Detailed outcomes of each self-healing scenario
   - Time taken for automatic restarts
   - Any failures or unexpected behaviors

6. Auto-scaling Test Results:
   - JMeter test configurations used
   - Graphs showing scaling behavior under different loads
   - HPA performance analysis

7. Performance Metrics:
   - Baseline performance data
   - Performance under various load conditions
   - Any bottlenecks identified

8. Issues and Resolutions:
   - List of any issues encountered during testing
   - Steps taken to resolve these issues

9. Recommendations:
   - Suggestions for improving application resilience
   - Potential optimizations for auto-scaling
   - Areas requiring further testing or development

10. Appendices:
    - Raw data from JMeter tests
    - Kubernetes and Prometheus configuration files
    - Any scripts used for testing

11. Sign-off:
    - Test completion date
    - Name and signature of tester(s)