# 🚀 Swiggy Clone Application | DevOps CI/CD Project

A complete DevOps project demonstrating the containerization, security scanning, CI/CD automation, and Kubernetes deployment of a **Swiggy Clone React application**.

The project uses **Jenkins** for CI/CD, **SonarQube** for code-quality analysis, **OWASP Dependency-Check** and **Trivy** for security scanning, **Docker** for containerization, and **Kubernetes/Argo CD** for deployment.

---

## 📌 Project Overview

This project demonstrates a practical DevOps workflow:

```text
Developer
    │
    ▼
 GitHub Repository
    │
    ▼
 Jenkins CI/CD
    │
    ├── Checkout Code
    ├── SonarQube Analysis
    ├── Quality Gate
    ├── npm install
    ├── OWASP Dependency Check
    ├── Trivy Filesystem Scan
    ├── Docker Build
    ├── Docker Image Push
    └── Trivy Image Scan
             │
             ▼
       Docker Registry
             │
             ▼
      Kubernetes / Argo CD
             │
             ▼
       Swiggy Application
```

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **React.js** | Frontend application |
| **Node.js / NPM** | Application runtime and package management |
| **GitHub** | Source-code management |
| **Jenkins** | CI/CD automation |
| **SonarQube** | Static code-quality analysis |
| **OWASP Dependency-Check** | Dependency vulnerability scanning |
| **Trivy** | Filesystem and Docker image security scanning |
| **Docker** | Application containerization |
| **Docker Hub** | Container image registry |
| **Kubernetes** | Container orchestration |
| **Argo CD** | GitOps-based Kubernetes deployment |
| **Terraform** | Infrastructure provisioning |

---

# 📂 Project Structure

```text
DevOps-Project-Swiggy-master/
│
├── ArgoCD/
│   ├── deployment.yml
│   ├── service.yml
│   └── README.md
│
├── Photos/
│   └── images.png
│
├── public/
│   └── index.html
│
├── src/
│   ├── Components/
│   │   ├── BestRes.css
│   │   ├── BestRest.jsx
│   │   ├── Footer.jsx
│   │   ├── Navigate.css
│   │   ├── Navigate.jsx
│   │   ├── OfferBanner.css
│   │   ├── OffersBanner.jsx
│   │   ├── RestaurentChain.css
│   │   ├── RestaurentChain.jsx
│   │   ├── RestaurentOnline.css
│   │   └── RestaurentOnline.jsx
│   │
│   ├── Photos/
│   ├── App.css
│   ├── App.js
│   ├── index.css
│   ├── index.js
│   └── bootstrap.min.css
│
├── Dockerfile
├── install.sh
├── jenkinsfile
├── package.json
├── package-lock.json
└── README.md
```

---

# ⚙️ Application Setup

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd DevOps-Project-Swiggy-master
```

---

## 2. Install Node.js Dependencies

```bash
npm install
```

---

## 3. Run the Application Locally

```bash
npm start
```

The React application runs on:

```text
http://localhost:3000
```

---

## 4. Create a Production Build

```bash
npm run build
```

The optimized production files are generated inside:

```text
build/
```

---

# 🐳 Docker Deployment

## Build the Docker Image

```bash
docker build -t swiggy .
```

Verify the image:

```bash
docker images
```

---

## Run the Container

```bash
docker run -d \
  --name swiggy \
  -p 3000:3000 \
  swiggy
```

Check the container:

```bash
docker ps
```

Access the application:

```text
http://<SERVER-IP>:3000
```

---

# 🔐 Docker Security Scan with Trivy

Scan the project filesystem:

```bash
trivy fs .
```

Scan the Docker image:

```bash
trivy image swiggy
```

Save the scan result:

```bash
trivy image swiggy > trivy.txt
```

---

# 🔎 SonarQube

SonarQube is used to analyze the source code for:

- Code-quality issues
- Bugs
- Code smells
- Security issues
- Maintainability problems

Start SonarQube using Docker:

```bash
docker run -d \
  --name sonar \
  -p 9000:9000 \
  sonarqube:lts-community
```

Access SonarQube:

```text
http://<SERVER-IP>:9000
```

The Jenkins pipeline uses the configured SonarQube server and scanner to perform the analysis.

---

# 🛡️ OWASP Dependency-Check

OWASP Dependency-Check is used to identify known vulnerabilities in application dependencies.

The Jenkins pipeline performs the dependency scan with:

```bash
dependencyCheck \
  --scan ./ \
  --disableYarnAudit \
  --disableNodeAudit
```

The generated report is published through Jenkins.

---

# 🔄 Jenkins CI/CD Pipeline

The project contains a `jenkinsfile` that automates the application lifecycle.

## Pipeline Stages

### 1. Clean Workspace

Removes files from the previous Jenkins build.

```groovy
stage('clean workspace')
```

### 2. Checkout Source Code

Downloads the application source code from GitHub.

```groovy
stage('Checkout from Git')
```

### 3. SonarQube Analysis

Performs static code analysis.

```groovy
stage("Sonarqube Analysis")
```

### 4. Quality Gate

Checks the SonarQube quality-gate result.

```groovy
stage("quality gate")
```

### 5. Install Dependencies

Installs the Node.js packages.

```bash
npm install
```

### 6. OWASP Dependency Scan

Checks application dependencies for known vulnerabilities.

### 7. Trivy Filesystem Scan

Scans the source directory.

```bash
trivy fs .
```

### 8. Docker Build

Builds the application image.

```bash
docker build -t swiggy .
```

### 9. Docker Image Push

Tags and pushes the image to Docker Hub.

Example:

```bash
docker tag swiggy <DOCKERHUB_USERNAME>/swiggy:latest
docker push <DOCKERHUB_USERNAME>/swiggy:latest
```

### 10. Trivy Docker Image Scan

Scans the generated Docker image.

```bash
trivy image <DOCKERHUB_USERNAME>/swiggy:latest
```

### 11. Deploy Container

Runs the Docker container.

```bash
docker run -d \
  --name swiggy \
  -p 3000:3000 \
  <DOCKERHUB_USERNAME>/swiggy:latest
```

---

# ☸️ Kubernetes Deployment

The project contains Kubernetes manifests under:

```text
ArgoCD/
```

## Deployment

The Kubernetes deployment creates two application replicas:

```yaml
replicas: 2
```

The application container exposes port:

```text
3000
```

Apply the deployment:

```bash
kubectl apply -f ArgoCD/deployment.yml
```

Check the deployment:

```bash
kubectl get deployment
```

Check the pods:

```bash
kubectl get pods
```

---

# 🌐 Kubernetes Service

The application is exposed using a Kubernetes `LoadBalancer` service.

```yaml
type: LoadBalancer
```

The service maps:

```text
Port 80 → Container Port 3000
```

Apply the service:

```bash
kubectl apply -f ArgoCD/service.yml
```

Check the service:

```bash
kubectl get svc
```

For a cloud Kubernetes cluster, obtain the external endpoint using:

```bash
kubectl get svc swiggy-app
```

Then access:

```text
http://<EXTERNAL-IP>
```

---

# 🔁 Argo CD Deployment

Argo CD is used to implement a GitOps deployment model.

The Kubernetes manifests are maintained inside:

```text
ArgoCD/
```

Main files:

```text
deployment.yml
service.yml
```

The expected GitOps workflow is:

```text
GitHub
   │
   ▼
Argo CD
   │
   ▼
Kubernetes Cluster
   │
   ├── Swiggy Pod 1
   └── Swiggy Pod 2
```

When the Kubernetes configuration stored in Git changes, Argo CD can synchronize the desired state with the Kubernetes cluster.

---

# 🖥️ Kubernetes Verification Commands

Check all resources:

```bash
kubectl get all
```

Check pods:

```bash
kubectl get pods
```

Check deployment:

```bash
kubectl get deployment
```

Check service:

```bash
kubectl get svc
```

Check pod logs:

```bash
kubectl logs <POD_NAME>
```

Describe a pod:

```bash
kubectl describe pod <POD_NAME>
```

---

# 🧹 Kubernetes Cleanup

Delete the application deployment:

```bash
kubectl delete -f ArgoCD/deployment.yml
```

Delete the service:

```bash
kubectl delete -f ArgoCD/service.yml
```

Or delete both manifests:

```bash
kubectl delete -f ArgoCD/
```

Verify:

```bash
kubectl get all
```

---

# 🧹 Docker Cleanup

Stop the application:

```bash
docker stop swiggy
```

Remove the container:

```bash
docker rm swiggy
```

Remove the image:

```bash
docker rmi swiggy
```

---

# 🚀 Complete DevOps Workflow

The complete implementation can be summarized as:

### Step 1
Develop the React Swiggy Clone application.

### Step 2
Push the source code to GitHub.

### Step 3
Jenkins pulls the source code.

### Step 4
Jenkins performs SonarQube code analysis.

### Step 5
Jenkins validates the SonarQube quality gate.

### Step 6
Application dependencies are installed.

### Step 7
OWASP Dependency-Check scans dependencies.

### Step 8
Trivy scans the source filesystem.

### Step 9
Docker builds the application image.

### Step 10
The Docker image is pushed to Docker Hub.

### Step 11
Trivy scans the Docker image.

### Step 12
The application is deployed using Docker or Kubernetes.

### Step 13
Argo CD manages the Kubernetes deployment using GitOps.

---

# 📊 DevOps Architecture

```text
                   ┌───────────────┐
                   │   Developer   │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │    GitHub     │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │    Jenkins    │
                   └───────┬───────┘
                           │
          ┌────────────────┼─────────────────┐
          ▼                ▼                 ▼
     ┌─────────┐     ┌───────────┐     ┌─────────┐
     │SonarQube│     │   OWASP   │     │  Trivy  │
     └─────────┘     └───────────┘     └─────────┘
                           │
                           ▼
                   ┌───────────────┐
                   │     Docker    │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │  Docker Hub   │
                   └───────┬───────┘
                           │
                           ▼
                   ┌───────────────┐
                   │   Argo CD     │
                   └───────┬───────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │    Kubernetes     │
                 │                   │
                 │  ┌─────┐ ┌─────┐ │
                 │  │Pod 1│ │Pod 2│ │
                 │  └─────┘ └─────┘ │
                 └─────────┬─────────┘
                           │
                           ▼
                    ┌────────────┐
                    │   Users    │
                    └────────────┘
```

---

# 🔧 Server Installation

The repository also contains:

```text
install.sh
```

The script installs/configures:

- Java 17
- Jenkins
- Docker
- SonarQube
- Trivy

Make the script executable:

```bash
chmod +x install.sh
```

Run it:

```bash
./install.sh
```

After installation, verify the services:

```bash
java --version
```

```bash
jenkins --version
```

```bash
docker --version
```

```bash
trivy --version
```

---

# ⚠️ Configuration Before Running Jenkins

Before executing the pipeline, configure the required tools and credentials in Jenkins.

The pipeline expects configured:

```text
JDK 17
Node.js
SonarQube Server
SonarQube Scanner
SonarQube Credentials
OWASP Dependency-Check
Docker credentials
Docker
```

The following names are referenced by the current Jenkinsfile and may need to match your Jenkins configuration:

```text
jdk17
node23
sonar-scanner
sonar-server
Sonar-token
DP-Check
docker-creds
```

Update the GitHub repository and Docker Hub image name in the Jenkinsfile if they differ from your environment.

---

# 🔐 Security Considerations

This project demonstrates multiple security checks within the CI/CD pipeline:

```text
Source Code
     │
     ├── SonarQube
     │
     ├── OWASP Dependency-Check
     │
     └── Trivy
           │
           ▼
      Docker Image
           │
           ▼
      Trivy Scan
```

Security scanning should be reviewed before deploying an image to a production environment.

---

# 📋 Prerequisites

Install the following before running the complete project:

- Git
- Node.js
- NPM
- Docker
- Jenkins
- Java
- SonarQube
- Trivy
- OWASP Dependency-Check
- Kubernetes cluster
- kubectl
- Argo CD
- Docker Hub account
- GitHub account

For AWS-based deployment, an AWS account and appropriate IAM permissions are also required.

---

# 🎯 Project Objectives

This project demonstrates practical experience with:

- React application deployment
- Git and GitHub
- CI/CD automation
- Jenkins pipelines
- Static code analysis
- Dependency vulnerability scanning
- Container security
- Docker image creation
- Docker Hub
- Kubernetes deployments
- Kubernetes services
- Argo CD
- GitOps
- DevSecOps practices

---

# 👨‍💻 Project Author

**Rahmanuddin**

GitHub:

`https://github.com/rahmanuddinmd`

---

# 📜 License

This project is intended for learning, practice, and demonstration of DevOps and DevSecOps concepts.

---

## ⭐ If this project helped you

Feel free to fork the repository, experiment with the pipeline, and extend the project with additional DevOps tools and cloud infrastructure.