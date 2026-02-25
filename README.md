# 🚀 CI/CD Pipeline: Jenkins → AWS ECR → AWS EKS

## 📌 Overview
This project implements a complete **CI/CD pipeline** for a containerized Java application using Jenkins.

On each pipeline run, the system automatically:

- 🔢 increments the Maven application version  
- 🏗 builds the Java artifact  
- 🐳 builds & tags a Docker image  
- 📦 pushes the image to **AWS ECR**  
- ☸️ deploys the new version to **AWS EKS**  
- 🔁 commits the updated version back to Git  

---

## 🧰 Technologies Used
- Jenkins (Pipeline as Code)  
- Docker  
- Kubernetes  
- AWS EKS (Elastic Kubernetes Service)  
- AWS ECR (Elastic Container Registry)  
- Java & Maven  
- Git  
- Linux  

---

## 🔄 Pipeline Stages

### 1️⃣ Version Increment (CI)
Automatically increments the patch version in `pom.xml` using Maven version plugins.

**Example:**  
`1.1.21 → 1.1.22`

The updated version is committed back to the repository.

---

### 2️⃣ Build Application (CI)

`mvn clean package`

Builds the executable Java application artifact.

---

### 3️⃣ Build & Push Docker Image (CI)

Pipeline performs:

- Docker image build  
- Dynamic tagging using `<version>-<build_number>`  
- Secure login to AWS ECR using Jenkins credentials  
- Push to private ECR repository  

**Example tag:**  
`1.1.22-15`

---

### 4️⃣ Deploy to AWS EKS (CD)

Deployment uses environment variable substitution:

`envsubst < kubernetes/deployment.yaml | kubectl apply -f -`

This dynamically injects the new image version into Kubernetes manifests and deploys to the EKS cluster.

---

### 5️⃣ Commit Version Update (CD)

After successful deployment:

- Jenkins commits the updated `pom.xml`  
- Pushes changes to the `jenkins-jobs` branch  
- Uses `[ci skip]` to prevent recursive pipeline triggers  

---

## 🔐 Credential Management

Sensitive credentials are securely stored in the **Jenkins Credentials Store**:

- AWS Access Key & Secret  
- ECR login credentials  
- Git repository credentials  

No secrets are stored in source code.

---

## 🎯 What This Project Demonstrates

- Automated semantic versioning  
- End-to-end CI/CD automation  
- Docker image lifecycle management  
- Secure credential handling  
- Kubernetes deployment automation  
- Immutable image tagging strategy  
- Git-based version tracking  
- Real-world DevOps deployment workflow  

---

## 🔮 Potential Improvements

- Add Helm chart templating  
- Implement automated testing stage  
- Add container security scanning (Trivy)  
- Integrate monitoring (Prometheus & Grafana)  
- Add Ingress & TLS for external access  

---

## 👤 Author

**Priscilla Salvador**  
Cloud & DevOps Engineer
