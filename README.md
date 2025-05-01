Here's a professional and comprehensive `README.md` tailored for your infrastructure GitOps and deployment workflows using GitHub Actions, Terraform, AWS EKS, ECR, and Helm:

---

# 🚀 Infrastructure GitOps on AWS with GitHub Actions

This repository automates the provisioning, deployment, and teardown of AWS infrastructure and Kubernetes workloads using GitHub Actions, Terraform, Helm, and ECR.

## 📦 Features

- **Infrastructure as Code (IaC)** using Terraform
- **CI/CD Workflows** with GitHub Actions
- **AWS EKS Cluster Management**
- **Container Image Build & Push** to AWS ECR
- **Kubernetes Deployment** via Helm
- **Ingress Controller** setup on EKS
- **Destruction Workflow** for resource cleanup
- **Manual Trigger Support** for controlled infra teardown

---

## 🗂️ Repository Structure

```
.
├── terraform/          # Terraform configuration for AWS infrastructure (EKS, VPC, etc.)
├── helm/               # Helm charts for Kubernetes app deployments
├── app/                # Dockerized application source code
├── .github/workflows/
│   ├── infra.yml       # Provisions or destroys infrastructure on AWS
│   └── deploy.yml      # Builds Docker image and deploys app to EKS
```

---

## 🔧 Workflows

### 1. 🏗 `infra.yml` – Infrastructure Provisioning and Destruction

#### ✅ Triggered On:

- Push to `new_branch` with changes inside the `terraform/` directory
- Manual trigger via `workflow_dispatch` (for destroy)

#### 📋 Jobs:

- `terraform`:  
  Initializes Terraform, plans, applies infrastructure on AWS, configures kubeconfig, and installs Ingress controller on success.
- `destroy`:  
  Manually triggered to:
  - Delete Kubernetes ingress controller
  - Empty AWS ECR repository
  - Destroy all Terraform-managed infrastructure

### 2. 🚢 `deploy.yml` – CI/CD for Application Deployment

#### ✅ Triggered On:

- Push to the `main` branch

#### 📋 Jobs:

- `BuildandPush`:  
  Builds Docker image from `./app`, tags it with the GitHub run number, and pushes it to AWS ECR.

- `DeployToEKS`:
  - Configures AWS credentials and kubeconfig
  - Creates Docker registry secret in the EKS cluster
  - Deploys the image using Helm chart from `helm/appchart`

---

## 🛠️ Setup Instructions

### ✅ Prerequisites

- AWS account with EKS, ECR, and S3 setup
- GitHub Secrets and Variables configured:

#### 🔐 Required Secrets:

| Key                     | Description          |
| ----------------------- | -------------------- |
| `AWS_ACCESS_KEY_ID`     | AWS IAM access key   |
| `AWS_SECRET_ACCESS_KEY` | AWS IAM secret key   |
| `REGISTRY`              | AWS ECR registry URI |

#### ⚙️ Required Variables:

| Key           | Description                      |
| ------------- | -------------------------------- |
| `AWS_REGION`  | AWS region (e.g., `us-east-1`)   |
| `EKS_CLUSTER` | EKS cluster name                 |
| `BUCKET`      | Terraform backend S3 bucket name |
| `ECR_REPO`    | AWS ECR repository name          |

---

## 🚀 Usage

### 🔨 Provision Infrastructure

> Triggered automatically on push to `new_branch` (inside `terraform/`), or manually run from Actions tab.

### 🐳 Build and Deploy App

> Push to `main` to build Docker image and deploy to EKS using Helm.

### 💣 Destroy Infrastructure

> Trigger the **Destroy Infra on AWS** job manually via GitHub Actions UI.

---

## 🧑‍💻 Maintainer

**Oyewunmi Olaleye**  
Website: [olaleye.com.ng](https://olaleye.com.ng)

---

Would you like me to generate a badge-style header (e.g., GitHub Actions status, Terraform format, etc.) or a project logo?
