
# Kubernetes-CICD-Monitoring Repository

**Description:**
This GitHub repository contains all the necessary configuration files and manifests to set up a Kubernetes cluster with Continuous Integration/Continuous Deployment (CI/CD) pipelines and monitoring components. The repository is designed to work with Minikube, a local Kubernetes cluster for development and testing purposes.

## Key Features:

- **Manifests for Kubernetes Cluster:** Includes Kubernetes manifests to create the core cluster components.
- **CI/CD Pipeline:** A Jenkins-based CI/CD pipeline is integrated into the cluster for automated application deployment.
- **Gitea Example:** Demonstrates the setup of Gitea, a self-hosted Git service. [Gitea](https://github.com/go-gitea/gitea) is a lightweight Git service that can be used for code version control.
- **Monitoring Stack:** Sets up monitoring using Prometheus and Grafana to monitor the cluster's health and performance.

## Getting Started:

To use the Kubernetes cluster created by this repository, follow these steps:

1. Connect the Docker environment of your host machine to Minikube:
   ```bash
   eval $(minikube docker-env)
   ```

2. Configure the `jenkinsCICD/jenkinsconf.yaml` and `jenkinsCICD/jobs/*/config` files:
   - Add your GitHub credentials in `jenkinsCICD/jenkinsconf.yaml`.
   - Set the URL to your Git repository in the job configurations located at `jenkinsCICD/jobs/*/config`.
   - Update Kubernetes manifests with the Minikube IP address for LoadBalancer services.

3. Build the Jenkins Docker image:
   ```bash
   docker build -t jenkins:jcasc ./jenkinsCICD
   ```

4. Deploy the infrastructure components to Minikube:
   ```bash
   kubectl apply -f ./jenkinsCICD
   ```

5. Get the address of the Jenkins service:
   ```bash
   minikube service list
   ```

6. Wait for Jenkins to scan your repository, perform artifact builds, and create Docker images.

7. After Jenkins completes its tasks, it will deploy the Gitea infrastructure with a PostgreSQL database in the Minikube cluster.

**Note:** This repository provides a comprehensive example of setting up a Kubernetes cluster for CI/CD and monitoring purposes. Customize the configurations to suit your specific project requirements.

## Contributions and Issue Reporting:

Feel free to contribute to this repository or report any issues you encounter. Your contributions and feedback are highly appreciated.

