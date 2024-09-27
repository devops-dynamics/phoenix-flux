# 1. Planning and Requirements (1 week)

## 1.1 Project Definition

### Project Scope: Small-Scale Self-Healing Infrastructure Demo

The project aims to create a small-scale demonstration of self-healing infrastructure using cloud technologies. Self-healing infrastructure refers to systems that can automatically detect and recover from failures without human intervention. This demo will showcase key aspects of self-healing on a manageable scale suitable for a college project.

Key aspects of the scope:
- Deploy a simple three-tier application (web, application, database)
- Implement basic monitoring and alerting
- Demonstrate automatic recovery from common failures
- Showcase auto-scaling capabilities

### Project Objectives

1. Demonstrate basic self-healing capabilities:
   - Automatic restart of failed containers
   - Redeployment of failed nodes
   - Load balancing to maintain availability during failures

2. Implement and showcase fundamental cloud-native technologies:
   - Containerization
   - Orchestration
   - Infrastructure as Code (IaC)
   - Monitoring and alerting

3. Gain practical experience with industry-standard tools and practices

4. Create a working demo that can detect and respond to at least two types of failures automatically

5. Document the entire process for educational purposes and future reference

## 1.2 Requirements Gathering

### Key Features

1. Monitoring:
   - Real-time tracking of system health and performance
   - Collection of relevant metrics (CPU usage, memory usage, response times, etc.)
   - Visualization of collected data
   - Alerting based on predefined thresholds

2. Auto-scaling:
   - Automatic adjustment of resources based on demand
   - Scaling of application instances to handle increased load
   - Scaling down during periods of low demand to optimize resource usage

3. Basic Error Recovery:
   - Automatic restart of failed containers
   - Redeployment of applications on healthy nodes in case of node failure
   - Self-healing of the cluster in case of minor failures

### Research on Free Tools

1. Cloud Platform: Oracle Cloud Infrastructure (OCI) Free Tier
   - Offers always-free resources suitable for small projects
   - Resources include:
     - 2 AMD-based Compute VMs
     - 2 Block Volumes (200 GB total)
     - 10 GB Object Storage
     - 10 Gbps networking
   - Provides a managed Kubernetes service (OKE) in the free tier
   - Allows for practical experience with a major cloud provider

2. Monitoring: Prometheus (open-source)
   - De facto standard for monitoring in cloud-native environments
   - Pull-based monitoring system with a flexible query language (PromQL)
   - Key features:
     - Multi-dimensional data model
     - Flexible alerting
     - Integrates well with Grafana for visualization
   - Active community and extensive documentation

3. Orchestration: Kubernetes (open-source)
   - Industry-standard container orchestration platform
   - Key features relevant to self-healing:
     - Automatic bin packing
     - Self-healing capabilities (restarts failed containers, replaces and reschedules containers when nodes die)
     - Horizontal scaling
     - Service discovery and load balancing
   - OCI offers managed Kubernetes (OKE), simplifying setup and management

4. Infrastructure as Code: Terraform (open-source)
   - Allows definition and provisioning of infrastructure using a declarative language
   - Supports multiple cloud providers, including OCI
   - Key benefits:
     - Version control for infrastructure
     - Reproducibility
     - Automation of provisioning process
   - Extensive documentation and active community support

### Additional Considerations

1. Application Stack:
   - Consider using a simple, well-documented stack for the demo application
   - Suggested stack: Python Flask (web), Redis (caching), PostgreSQL (database)
   - All components have official Docker images available

2. Version Control:
   - Utilize GitHub for version control and project management
   - Take advantage of GitHub Actions for CI/CD pipelines

3. Visualization:
   - Consider adding Grafana for better visualization of Prometheus metrics
   - Grafana offers a feature-rich free tier

4. Load Testing:
   - Plan to use Apache JMeter (free, open-source) for load testing to demonstrate auto-scaling

## Deliverable: Project Requirements Document

The project requirements document should include:

1. Project Overview
   - Brief description of self-healing infrastructure
   - Project goals and objectives
   - Scope and limitations

2. System Architecture
   - High-level diagram of the proposed system
   - Description of each component and its role

3. Functional Requirements
   - Detailed list of features to be implemented
   - Acceptance criteria for each feature

4. Non-Functional Requirements
   - Performance expectations
   - Scalability requirements
   - Reliability and availability targets

5. Technology Stack
   - List of all tools and technologies to be used
   - Justification for each choice

6. Implementation Plan
   - High-level timeline
   - Major milestones

7. Testing Strategy
   - Types of tests to be performed
   - Tools to be used for testing

8. Deliverables
   - List of all project deliverables
   - Format and submission guidelines

9. Constraints and Assumptions
   - Budget constraints (free tier usage)
   - Time limitations
   - Assumptions made during planning