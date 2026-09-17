# Swiggy Clone Application - DevOps CI/CD Project

## 📌 Project Overview

This project demonstrates an end-to-end **DevOps CI/CD pipeline** for a Swiggy Clone web application.

The application source code is hosted on GitHub and the complete delivery workflow is automated using Jenkins. The pipeline performs source-code checkout, dependency installation, SonarQube code analysis, Quality Gate validation, OWASP Dependency-Check, Trivy security scanning, Docker image creation, Docker Hub publishing, and deployment to an AWS EC2 instance.

## 🏗️ Architecture

```text
Developer
    |
    v
GitHub Repository
    |
    v
Jenkins CI/CD Pipeline
    |
    +--------------------+
    |                    |
    v                    v
SonarQube             Security Scans
    |                  - OWASP Dependency-Check
    |                  - Trivy FS Scan
    |                  - Trivy Image Scan
    |
    v
Docker Build
    |
    v
Docker Hub
    |
    v
AWS EC2
    |
    v
Docker Container
    |
    v
Swiggy Clone Application
    |
    v
Port 3000
```

## 🔗 Repository

GitHub:

`https://github.com/rahmanuddinmd/Swiggy-Clone-Application-DevOps-CI-CD-Project.git`

Docker Hub image:

`rahmanuddinmd17/swiggy:latest`

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| GitHub | Source code management |
| Jenkins | CI/CD automation |
| Java 21 | Jenkins runtime |
| Node.js 26 | Jenkins Node.js build environment |
| npm | Dependency management |
| SonarQube | Static code analysis |
| OWASP Dependency-Check | Dependency vulnerability analysis |
| Trivy | Filesystem and container image security scanning |
| Docker | Application containerization |
| Docker Hub | Container image registry |
| AWS EC2 | Application deployment server |
| Linux/Ubuntu | Server operating system |

## ☁️ AWS Infrastructure

The application is deployed on an AWS EC2 instance.

### EC2 Configuration

- Region: `ap-southeast-2`
- Instance type: `t2.large`
- Operating system: Ubuntu
- Root volume: 30 GB gp3
- Docker: Installed
- Jenkins: Installed
- SonarQube: Running in Docker
- Trivy: Installed

### Application Ports

| Port | Service |
|---:|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3000 | Swiggy application |
| 8080 | Jenkins |
| 9000 | SonarQube |

> For production environments, restrict public access to administrative ports such as SSH, Jenkins, and SonarQube.

## 🔄 CI/CD Pipeline Stages

### 1. Clean Workspace

Jenkins removes files from the previous build.

```groovy
cleanWs()
```

### 2. Checkout from Git

The pipeline checks out the `main` branch from GitHub.

```text
https://github.com/rahmanuddinmd/Swiggy-Clone-Application-DevOps-CI-CD-Project.git
```

### 3. Verify Tools

The pipeline verifies:

```bash
java --version
node --version
npm --version
docker --version
trivy --version
```

### 4. Install Dependencies

Node.js dependencies are installed using:

```bash
npm install --legacy-peer-deps
```

### 5. SonarQube Analysis

SonarQube analyzes the project using:

```bash
sonar-scanner
```

Project:

```text
Project Name: Swiggy
Project Key : Swiggy
```

### 6. Quality Gate

Jenkins waits for the SonarQube Quality Gate result.

The successful pipeline run reported:

```text
Quality gate is 'OK'
```

### 7. OWASP Dependency-Check

The pipeline checks project dependencies for known vulnerabilities.

```groovy
dependencyCheck(
    additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit',
    odcInstallation: 'DP-Check'
)
```

The generated report is:

```text
dependency-check-report.xml
```

### 8. Trivy Filesystem Scan

The complete workspace is scanned:

```bash
trivy fs . \
    --format table \
    --output trivyfs.txt
```

### 9. Docker Image Build

The application is containerized:

```bash
docker build \
    -t rahmanuddinmd17/swiggy:latest .
```

### 10. Trivy Docker Image Scan

The generated Docker image is scanned:

```bash
trivy image \
    --format table \
    --output trivy.txt \
    rahmanuddinmd17/swiggy:latest
```

### 11. Docker Hub Push

Jenkins authenticates with Docker Hub using the Jenkins credential:

```text
docker-creds
```

Then pushes:

```bash
docker push rahmanuddinmd17/swiggy:latest
```

### 12. Deploy to Docker Container

The existing container is removed and the latest image is pulled:

```bash
docker rm -f swiggy 2>/dev/null || true

docker pull rahmanuddinmd17/swiggy:latest
```

The application is started:

```bash
docker run -d \
    --name swiggy \
    --restart unless-stopped \
    -p 3000:3000 \
    rahmanuddinmd17/swiggy:latest
```

### 13. Deployment Verification

Jenkins verifies that the container is running:

```bash
docker ps | grep swiggy
```

Then it checks the application:

```bash
curl -f -I http://localhost:3000
```

A successful deployment returns:

```text
HTTP/1.1 200 OK
```

## 🐳 Docker

The Docker image is:

```text
rahmanuddinmd17/swiggy:latest
```

The application container is:

```text
swiggy
```

Port mapping:

```text
EC2 Port 3000 -> Container Port 3000
```

Check the running container:

```bash
docker ps
```

View application logs:

```bash
docker logs swiggy
```

Stop the container:

```bash
docker stop swiggy
```

Start the container:

```bash
docker start swiggy
```

Remove the container:

```bash
docker rm -f swiggy
```

## 🔍 SonarQube

SonarQube runs as a Docker container:

```text
Container: sonar
Port: 9000
```

Check:

```bash
docker ps
```

Open SonarQube:

```text
http://<EC2-PUBLIC-IP>:9000
```

The Jenkins pipeline successfully completed SonarQube analysis and received an `OK` Quality Gate in the reviewed build.

## 🔐 Security Scanning

### OWASP Dependency-Check

Checks third-party project dependencies against known vulnerabilities.

### Trivy Filesystem Scan

Scans the project workspace for vulnerabilities and secrets.

Output:

```text
trivyfs.txt
```

### Trivy Image Scan

Scans the Docker image for vulnerabilities.

Output:

```text
trivy.txt
```

## 📊 Pipeline Reports

Jenkins archives the following reports:

```text
trivyfs.txt
trivy.txt
dependency-check-report.xml
```

These reports can be downloaded from the Jenkins build under archived artifacts when they are generated.

## 📁 Recommended Project Structure

```text
Swiggy-Clone-Application-DevOps-CI-CD-Project/
│
├── src/
├── public/
├── package.json
├── package-lock.json
├── Dockerfile
├── Jenkinsfile
├── README.md
│
└── ...
```

## ⚙️ Jenkins Configuration

The Jenkins pipeline uses these configured tools:

```text
JDK:
    jdk21

NodeJS:
    node26

SonarQube Scanner:
    sonar-scanner

Dependency-Check:
    DP-Check
```

Jenkins credentials:

```text
Docker Hub:
    docker-creds

SonarQube:
    Sonar-token
```

Do not store Docker passwords, access tokens, AWS keys, or other secrets directly inside the Jenkinsfile.

## 📜 Jenkins Pipeline

The pipeline follows this workflow:

```text
Clean Workspace
       ↓
Git Checkout
       ↓
Verify Tools
       ↓
Install Dependencies
       ↓
SonarQube Analysis
       ↓
Quality Gate
       ↓
OWASP Dependency Check
       ↓
Trivy Filesystem Scan
       ↓
Docker Build
       ↓
Trivy Image Scan
       ↓
Docker Hub Push
       ↓
Deploy Container
       ↓
Verify Deployment
```

## 🚀 How to Run the Project

### Step 1: Clone the Repository

```bash
git clone https://github.com/rahmanuddinmd/Swiggy-Clone-Application-DevOps-CI-CD-Project.git
cd Swiggy-Clone-Application-DevOps-CI-CD-Project
```

### Step 2: Install Dependencies

```bash
npm install --legacy-peer-deps
```

### Step 3: Run Locally

```bash
npm start
```

The application runs on:

```text
http://localhost:3000
```

### Step 4: Build Docker Image

```bash
docker build -t rahmanuddinmd17/swiggy:latest .
```

### Step 5: Run Docker Container

```bash
docker run -d \
    --name swiggy \
    -p 3000:3000 \
    rahmanuddinmd17/swiggy:latest
```

### Step 6: Verify

```bash
docker ps
curl -I http://localhost:3000
```

Expected response:

```text
HTTP/1.1 200 OK
```

## 🧪 Jenkins Build Result

The reviewed pipeline run demonstrated that the following parts of the CI/CD process completed successfully:

```text
Git Checkout          SUCCESS
Tool Verification     SUCCESS
NPM Installation      SUCCESS
SonarQube Analysis    SUCCESS
Quality Gate          SUCCESS
Trivy FS Scan         SUCCESS
Docker Build          SUCCESS
Trivy Image Scan      SUCCESS
Docker Hub Push       SUCCESS
Container Deployment  SUCCESS
Deployment Check      SUCCESS
```

The deployed container was running as:

```text
rahmanuddinmd17/swiggy:latest
```

and the application health check returned:

```text
HTTP/1.1 200 OK
```

## ⚠️ Known Items

### OWASP NVD API Configuration

The reviewed build showed that Dependency-Check could not update NVD data because the configured NVD API key was empty:

```text
Invalid API Key, length of 0
Error updating the NVD Data
No documents exist
```

The Dependency-Check configuration should therefore be completed with a valid NVD API key stored securely in Jenkins.

### Node.js Compatibility

SonarQube reported that Node.js 26 is not its recommended version for the JavaScript analyzer and listed Node.js 16 and 18 as recommended for that analyzer version.

### Docker Base Image

The current Dockerfile uses:

```dockerfile
FROM node:16
```

Trivy reported that the Debian 10 base associated with this image is no longer supported.

These items should be reviewed before treating the configuration as production-ready.

## 🔒 Security Best Practices

- Use Jenkins Credentials for secrets.
- Never commit AWS access keys or secret keys.
- Use Docker Hub Personal Access Tokens instead of account passwords.
- Restrict AWS Security Group access where possible.
- Keep Jenkins and SonarQube administrative interfaces private or IP-restricted.
- Regularly update the Docker base image.
- Keep Trivy and Dependency-Check vulnerability databases updated.
- Review and remediate high and critical npm vulnerabilities.
- Add a `.dockerignore` file to avoid copying unnecessary files such as `node_modules`.

## 📈 Future Improvements

Possible improvements include:

- Multi-stage Docker build
- Smaller production Docker image
- Updated Node.js base image
- `.dockerignore`
- NVD API key configuration through Jenkins Credentials
- Automated vulnerability thresholds
- Docker image versioning using Jenkins build numbers
- HTTPS with a reverse proxy
- AWS Application Load Balancer
- Infrastructure provisioning with Terraform
- Kubernetes/EKS deployment
- Prometheus and Grafana monitoring
- Automated rollback
- GitHub webhook-triggered Jenkins builds

## 👨‍💻 Author

**Rahmanuddin**

GitHub:

`https://github.com/rahmanuddinmd`

---

## ⭐ Project Summary

This project demonstrates a complete DevOps CI/CD implementation for a React-based Swiggy Clone application:

**GitHub → Jenkins → SonarQube → OWASP → Trivy → Docker → Docker Hub → AWS EC2 → Docker Container → Application**

The pipeline automates the software delivery process from source-code checkout through security validation, containerization, image publishing, deployment, and application verification.
