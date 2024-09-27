# Phoenix Flux: Self-Healing Infrastructure Demo

## 🌟 Overview

Phoenix Flux is a cutting-edge demonstration of self-healing infrastructure using cloud-native technologies. This project showcases a robust, scalable, and resilient application deployment that can automatically recover from failures and adjust to varying loads without human intervention.

## 🚀 Key Features

- **Self-Healing**: Automatic recovery from common failure scenarios
- **Auto-Scaling**: Dynamic resource adjustment based on demand
- **Cloud-Native**: Leverages Kubernetes for container orchestration
- **Infrastructure as Code**: Uses Terraform for reproducible infrastructure
- **Comprehensive Monitoring**: Real-time insights with Prometheus and Grafana

## 🛠 Tech Stack

- **Cloud Platform**: Oracle Cloud Infrastructure (OCI)
- **Container Orchestration**: Kubernetes (OKE)
- **Application**: Python Flask
- **Database**: PostgreSQL
- **Infrastructure as Code**: Terraform
- **Monitoring**: Prometheus & Grafana
- **Version Control**: Git

## 📂 Project Structure

```
.
├── docs/                 # Project documentation
├── infrastructure/       # Infrastructure as Code
│   ├── kubernetes/       # Kubernetes configuration files
│   └── terraform/        # Terraform scripts for OCI
├── monitoring/           # Monitoring configuration
│   ├── grafana/          # Grafana dashboards
│   └── prometheus/       # Prometheus configuration
├── scripts/              # Utility scripts
├── src/                  # Application source code
│   ├── app/              # Flask application
│   └── tests/            # Unit tests
├── .gitignore
├── README.md
└── requirements.txt
```

## 🚦 Getting Started

### Prerequisites

- OCI Account with Free Tier access
- Terraform >= 0.12
- kubectl
- Docker
- Python 3.8+

### Setup

1. Clone the repository:
   ```
   git clone https://github.com/yourusername/phoenix-flux.git
   cd phoenix-flux
   ```

2. Set up OCI CLI and configure your credentials.

3. Initialize and apply Terraform configuration:
   ```
   cd infrastructure/terraform
   terraform init
   terraform apply
   ```

4. Deploy the application to Kubernetes:
   ```
   cd ../../
   ./scripts/deploy.sh
   ```

5. Access the application:
   ```
   kubectl get services
   ```
   Use the external IP provided to access the application.

## 📊 Monitoring

Access Prometheus and Grafana dashboards:

1. Port forward Prometheus:
   ```
   kubectl port-forward svc/prometheus-server 9090:9090
   ```
   Access at `http://localhost:9090`

2. Port forward Grafana:
   ```
   kubectl port-forward svc/grafana 3000:3000
   ```
   Access at `http://localhost:3000`

## 🧪 Testing

Run unit tests:
```
python -m pytest src/tests
```

For load testing and demonstrating auto-scaling, refer to `docs/4_testing.md`.

## 📚 Documentation

Detailed documentation is available in the `docs/` directory:

- `1_planning_and_requirements.md`: Project planning and requirements
- `2_design.md`: System design and architecture
- `3_implementation.md`: Implementation details
- `4_testing.md`: Testing procedures and results
- `5_documentation_and_presentation.md`: Documentation guidelines
- `6_submission_and_demo.md`: Project submission and demo instructions
- `architecture.md`: Detailed system architecture
- `setup.md`: Comprehensive setup guide
- `user_guide.md`: User manual for the application

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgements

- Oracle Cloud for providing free-tier resources
- The Kubernetes, Terraform, and Flask communities for their excellent documentation and tools

Happy coding! 🚀
