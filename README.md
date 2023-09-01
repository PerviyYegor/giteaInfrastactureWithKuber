Description:
This GitHub repository contains all the necessary configuration files and manifests to set up a Kubernetes cluster with Continuous Integration/Continuous Deployment (CI/CD) pipelines and monitoring components. The repository is designed to work with Minikube, a local Kubernetes cluster for development and testing purposes.

Features:

Manifests for Kubernetes Cluster: Includes Kubernetes manifests to create the core cluster components.
CI/CD Pipeline: A Jenkins-based CI/CD pipeline is integrated into the cluster for automated application deployment.
Gitea Example: Demonstrates the setup of Gitea, a self-hosted Git service. Gitea is a lightweight Git service that can be used for code version control.
Monitoring Stack: Sets up monitoring using Prometheus and Grafana to monitor the cluster's health and performance.
Getting Started:
To use the Kubernetes cluster created by this repository, follow these steps:

Connect the Docker environment of your host machine to Minikube:

bash
Copy code
eval $(minikube docker-env)
Configure the jenkinsCICD/jenkinsconf.yaml and jenkinsCICD/jobs/*/config files:

Add your GitHub credentials in jenkinsCICD/jenkinsconf.yaml.
Set the URL to your Git repository in the job configurations located at jenkinsCICD/jobs/*/config.
Update Kubernetes manifests with the Minikube IP address for LoadBalancer services.
Build the Jenkins Docker image:

bash
Copy code
docker build -t jenkins:jcasc ./jenkinsCICD
Deploy the infrastructure components to Minikube:

bash
Copy code
kubectl apply -f ./jenkinsCICD
Get the address of the Jenkins service:

bash
Copy code
minikube service list
Wait for Jenkins to scan your repository, perform artifact builds, and create Docker images.

After Jenkins completes its tasks, it will deploy the Gitea infrastructure with a PostgreSQL database in the Minikube cluster.

Note: This repository provides a comprehensive example of setting up a Kubernetes cluster for CI/CD and monitoring purposes. Customize the configurations to suit your specific project requirements.
