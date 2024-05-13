Apologies for the oversight. Let's include the `kubectl` installation command in the documentation:

---

# Node.js Application Deployment Documentation

This document provides detailed instructions on how to deploy the Node.js application to a Kubernetes cluster using Docker, Minikube, Argo CD, Helm charts, and GitHub Actions for CI/CD.

## Step 1: Setup Development Environment

### Docker Installation

```bash
sudo apt update
sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock
```

### Minikube Installation

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
minikube start --driver=docker
```

### kubectl Installation

```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2 curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

## Step 2: Deploy Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Step 3: Helm Installation

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Step 4: Set Up GitHub Actions for CI/CD

1. Configure GitHub Actions workflows in your GitHub repository to automate the Docker image build and push process upon changes.
   
2. Make sure to add DockerHub credentials securely to GitHub repository secrets for access during CI/CD workflows.

## Step 5: Deployment Process

### GitHub Repository Setup

1. Create a new GitHub repository in your account.

2. Push the following files to your GitHub repository:
   - Node.js application code.
   - Dockerfile.
   - Helm chart for the application.
   - Argo CD YAML files for application deployment.

### Docker Image on DockerHub

Ensure the Docker image for the Node.js application is available on DockerHub. Your DockerHub repository URL is: [Your DockerHub Repository URL](https://hub.docker.com/repository/docker/28cloud/nodeaapp/general).

To access your DockerHub repository URL using `kubectl`, run the following command:

```bash
kubectl port-forward svc/helmrelease-nodeapp --address 0.0.0.0 3000:3000
```

### Argo CD Deployment

1. Provide Argo CD with necessary details:
   - Git repository URL.
   - Path to `values.yaml` file.
   - Path to Helm chart directory.

2. Argo CD will automatically deploy the application to the Kubernetes cluster upon changes to the Helm chart or application code triggered by GitHub Actions.

## Step 6: Accessing Deployed Resources

1. Access the deployed Node.js application:
   ```bash
   kubectl port-forward svc/helmrelease-nodeapp --address 0.0.0.0 3000:3000
   ```
   The application will be accessible at `http://localhost:3000`.

2. Access the Argo CD dashboard:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd --address 0.0.0.0 8080:443
   ```
   The Argo CD dashboard will be accessible at `https://localhost:8080`.

## Conclusion

Congratulations! You have successfully deployed the Node.js application to Kubernetes using Docker, Minikube, Argo CD, Helm charts, and GitHub Actions. Continuous integration and continuous deployment (CI/CD) pipelines ensure efficient development workflows and maintain the desired state of your application.

---

