# Sample Blue-Green Deployment with Jenkins on Minikube

This project demonstrates a **Blue-Green Deployment** pipeline using:
- Jenkins deployed on **Minikube** as a Kubernetes pod
- A dummy **Node.js application**
- **Docker** for image builds
- **Jenkins Pipeline** to automate deployment and traffic switch

---

## 🛠 Prerequisites

Ensure the following tools are installed:

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-docker/)
- [Git](https://git-scm.com/)
- [Jenkins CLI (Optional)](https://www.jenkins.io/doc/book/managing/cli/)

---

## 📁 Project Structure

GitHub Repo: [https://github.com/bhargavinaini/Sample-Blue-Green-deployment](https://github.com/bhargavinaini/Sample-Blue-Green-deployment)

├── Jenkinsfile # Jenkins pipeline script 

├── nodejs-app/

│ ├── app.js # Simple nodejs app

│ ├── Dockerfile # Docker build file

│ └── blue-dep.yaml # Kubernetes deployment for Blue version

| ├── green-dep.yaml # Kubernetes deployment for Green version

├── jenkins-deployment.yaml # Jenkins pod with Docker access

└── README.md

## 🚀 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/bhargavinaini/Sample-Blue-Green-deployment.git
cd Sample-Blue-Green-deployment 
```
 ### 2. Start Minikube
 ```bash
minikube start
```
### 3. Enable Minikube Docker Environment
```bash
eval $(minikube docker-env)
```
### 4. Build & push Docker Image
```bash
cd nodejs-app
docker build -t bhargavinaini21/nodejs:blue .
docker push bhargavinaini21/nodejs:blue
docker build -t bhargavinaini21/nodejs:green .
docker push bhargavinaini21/nodejs:blue
```
## 🧰 Deploy Jenkins on Minikube

### 1. Create Jenkins Deployment & Service
```bash
kubectl apply -f jenkins-pod/jenkins-dep.yaml
```
### 2. Access Jenkins
``` bash
minikube service jenkins-service
```
### 4. Unlock Jenkins
``` bash
kubectl exec --namespace default -it <jenkins-pod-name> -- cat /var/jenkins_home/secrets/initialAdminPassword
```
### 5.Deploy app
```bash
kubectl apply -f blue-dep.yml"
```
### 6. Switch Traffic
```bash
kubectl patch service nodejs-service -p '{\"spec\":{\"selector\":{\"app\":\"nodejs\", \"version\":\"${green}\"}}}'
```

## ⚙️ Jenkins CI/CD Pipeline

1. Create New Pipeline Job in Jenkins.

2. Use the GitHub repo URL:
https://github.com/bhargavinaini/Sample-Blue-Green-deployment.git

3. Add a Jenkinsfile from the directory.

4. Run the pipeline to automate the build, deploy, and switch process.
          
