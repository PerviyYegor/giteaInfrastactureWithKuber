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

1. Connect the Docker environment of your host machine to Minikube(after creation of Minikube with minikube start):

   ```bash
   eval $(minikube docker-env)
   ```
2. Configure the `jenkinsCICD/jenkinsconf.yaml` and `jenkinsCICD/jobs/giteaCICD/config` files:

   - Add your GitHub credentials in `jenkinsCICD/jenkinsconf.yaml`.
   - Set the URL to your Git repository in the job configurations located at `jenkinsCICD/jobs/giteaCICD/config`.
     (NOTE: in your repository should be Jenkins file in root of project. Example for Gitea is in /Jenkinsfile-example file, also there should be kubernetes manifests for raise of infrasracture and Dockerfile for build gitea container)
   - Update Kubernetes manifests with the Minikube IP address for LoadBalancer services.
3. Build the Jenkins Docker image:

   ```bash
   docker build -t jenkins:jcasc ./jenkinsCICD
   ```
4. Deploy the infrastructure components to Minikube:

   ```bash
   kubectl apply -f ./jenkinsCICD/jenkinsK8sCICD.yaml
   ```
5. Create and get Jenkins service to have access to Jenkins WebUI :

   ```bash
   kubect kubectl expose deployment jenkins-deployment --type=LoadBalancer --external-ip=$(minikube ip)
   kubectl get services
   ```
6. Wait for Jenkins to scan your repository, perform artifact builds, and create Docker images.
7. After Jenkins completes its tasks, it will deploy the Gitea infrastructure with a PostgreSQL database in the Minikube cluster.
8. To expose Gitea to external ip you also should execute (but only after success creation of gitea and postgresql pods):

   ```
   kubectl expose deployment gitea-deployment --type=LoadBalancer --namespace=gitea-app --external-ip=$(minikube ip)
   ```

**Monitoring Stack Components:**
For the monitoring components, we have used manifests from the following repositories:

- [kubernetes-grafana](https://github.com/bibinwilson/kubernetes-grafana)
- [kube-state-metrics-configs](https://github.com/devopscube/kube-state-metrics-configs)
- [kubernetes-prometheus](https://github.com/techiescamp/kubernetes-prometheus)

**Note:** This repository provides a comprehensive example of setting up a Kubernetes cluster for CI/CD and monitoring purposes. Customize the configurations to suit your specific project requirements.

## Contributions and Issue Reporting:

Feel free to contribute to this repository or report any issues you encounter. Your contributions and feedback are highly appreciated.
