# 🚀 CI/CD Pipeline with Jenkins, AWS ECR, and EKS

## 📌 Project Overview

This project implements a complete CI/CD pipeline for a Java application.

The pipeline automatically:

- Increments the Maven application version
- Builds the Java artifact
- Builds and pushes a Docker image to AWS ECR
- Deploys the new version to an AWS EKS cluster
- Commits the updated version back to Git

---

## 🏗 Technologies Used

- Jenkins 
- Docker
- Kubernetes
- AWS EKS (Elastic Kubernetes Service)
- AWS ECR (Elastic Container Registry)
- Java 
- Maven
- Git
- Linux

---

## 🔄 Pipeline Stages Breakdown

### 1️⃣ Increment Version (CI)

Uses Maven build-helper and versions plugin to automatically increment the patch version in `pom.xml`.

Example:
1.1.21 → 1.1.22

The updated version is committed back to Git.

---

### 2️⃣ Build Java Application (CI)

```
mvn clean package
```

Builds the executable Spring Boot JAR.

---

### 3️⃣ Build & Push Docker Image to AWS WXE (CI)

- Builds Docker image
- Tags image using:
  <version>-<build_number>
- Logs into AWS ECR using Jenkins credentials  
- Pushes image to private AWS ECR repository

Example image tag:
1.1.2-15

---

### 4️⃣ Deploy to EKS Cluster (CD)

Uses:

```
envsubst < kubernetes/deployment.yaml | kubectl apply -f -
```

This dynamically adds the image tag into Kubernetes manifests and deploys to EKS.

---

### 5️⃣ Commit Version Update Back to Repository (CD)

After successful deployment:

- Jenkins commits the updated `pom.xml`
- Pushes changes to the`jenkins-jobs` branch
- Uses `[ci skip]` to prevent recursive builds

---

## 🔐 Credential Management
The pipeline securely uses:

- AWS Access Key
- AWS Secret Key
- ECR credentials
- GitLab credentials

All secrets are stored in Jenkins Credentials Store.

---

## 🎯 What This Project Demonstrates

- Automated semantic versioning
- CI/CD automation
- Docker image lifecycle management
- Secure credential handling
- Kubernetes deployment automation
- Immutable image tagging strategy
- Git-based version tracking