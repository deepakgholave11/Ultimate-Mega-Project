# 🏦 BankApp — Production-Style DevOps Pipeline on AWS EKS

![CI](https://img.shields.io/badge/CI-Jenkins-blue?logo=jenkins) ![CD](https://img.shields.io/badge/CD-ArgoCD-orange?logo=argo) ![Cloud](https://img.shields.io/badge/Cloud-AWS%20EKS-yellow?logo=amazonaws) ![IaC](https://img.shields.io/badge/IaC-Terraform-purple?logo=terraform) ![Security](https://img.shields.io/badge/Security-Trivy%20%7C%20SonarQube-green)

A complete end-to-end DevOps implementation for a Java-based Banking Application — covering CI/CD automation, containerization, Kubernetes orchestration, infrastructure provisioning, and security scanning.

---

## 📐 Architecture Overview

```
Developer Push
      │
      ▼
┌─────────────┐     ┌──────────────────────────────────────────────────────┐
│   GitHub    │────▶│              Jenkins CI Pipeline                      │
│  (Source)   │     │  Maven Build → Trivy FS → SonarQube → Quality Gate   │
└─────────────┘     │  → Nexus → Docker Build → Trivy Image → DockerHub    │
                    └──────────────────┬───────────────────────────────────┘
                                       │ Updates manifest.yaml (GitOps)
                                       ▼
                    ┌──────────────────────────────────┐
                    │         GitHub (CD Repo)          │
                    │     manifest.yaml (image tag)     │
                    └──────────────┬───────────────────┘
                                   │ Watches repo
                                   ▼
                    ┌──────────────────────────────────┐
                    │         Jenkins CD Pipeline       │
                    │   kubectl apply → EKS Deploy      │
                    └──────────────┬───────────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────────┐
                    │         AWS EKS Cluster           │
                    │  Namespace: webapps               │
                    │  Pods + HPA + Service             │
                    └──────────────────────────────────┘
                    
Infrastructure provisioned via Terraform (bankapp-terraform)
```

---

## 🗂️ Repository Structure

```
Ultimate-Mega-Project/
├── bankapp-ci/              # Java Spring Boot application + CI Jenkinsfile
│   ├── src/                 # Application source code
│   ├── Dockerfile           # Container image definition
│   ├── pom.xml              # Maven build config
│   └── Jenkinsfile          # CI Pipeline (Build → Scan → Push)
│
├── bankapp-cd/              # Kubernetes manifests + CD Jenkinsfile
│   ├── Manifest/
│   │   ├── manifest.yaml    # Deployment + Service (image tag auto-updated)
│   │   └── HPA.yaml         # Horizontal Pod Autoscaler
│   └── Jenkinsfile          # CD Pipeline (kubectl apply to EKS)
│
├── bankapp-terraform/       # Infrastructure as Code
│   ├── main.tf              # EKS cluster + VPC provisioning
│   ├── variables.tf
│   └── outputs.tf
│
└── Commands.txt             # Reference commands used during setup
```

---

## ⚙️ CI Pipeline — Stages

| Stage | Tool | Description |
|---|---|---|
| Git Checkout | Jenkins + GitHub | Pull latest source code |
| Compile | Maven 3 | Compile Java source |
| Unit Testing | Maven | Run test suite |
| Filesystem Scan | Trivy | Scan source for vulnerabilities |
| Code Analysis | SonarQube | Static code analysis |
| Quality Gate | SonarQube | Fail pipeline if quality threshold not met |
| Build Package | Maven | Package JAR artifact |
| Publish Artifact | Nexus | Store versioned artifact in Nexus repository |
| Docker Build | Docker | Build image with tag `v${BUILD_NUMBER}` |
| Image Scan | Trivy | Scan Docker image for CVEs |
| Docker Push | DockerHub | Push versioned image to registry |
| Update Manifest | Git + sed | Update image tag in manifest.yaml via GitOps |

---

## 🚀 CD Pipeline — Stages

| Stage | Tool | Description |
|---|---|---|
| Git Checkout | Jenkins + GitHub | Pull updated CD manifests |
| Kubernetes Deploy | kubectl + EKS | Apply deployment and HPA to `webapps` namespace |
| Verification | kubectl | Get pods and service status |

---

## 🏗️ Infrastructure (Terraform)

- **Provider:** AWS (ap-south-1)
- **Resources provisioned:**
  - VPC with public/private subnets
  - EKS Cluster with managed node groups
  - IAM roles and security groups
  - EC2 instances for Jenkins, SonarQube, Nexus

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| CI/CD | Jenkins |
| Version Control | GitHub |
| Build Tool | Maven 3 |
| Code Quality | SonarQube |
| Security Scanning | Trivy |
| Artifact Registry | Nexus Repository |
| Containerization | Docker |
| Container Registry | DockerHub |
| Orchestration | Kubernetes (AWS EKS) |
| Auto Scaling | Horizontal Pod Autoscaler (HPA) |
| Infrastructure | Terraform |
| Cloud | AWS (EKS, EC2, VPC, IAM) |
| Notifications | Email via Jenkins emailext |

---

## 🔐 Security Highlights

- **Trivy FS Scan** — scans filesystem before build to catch dependency vulnerabilities
- **SonarQube Quality Gates** — blocks deployment if code quality thresholds fail
- **Trivy Image Scan** — scans final Docker image before pushing to registry
- **Kubernetes RBAC** — restricted kubeconfig access for CD pipeline
- **Jenkins Credentials Store** — no hardcoded secrets in pipeline code

---

## 📦 Prerequisites

- AWS account with EKS permissions
- Jenkins server with plugins: Maven, SonarQube, Docker, Kubernetes, emailext
- SonarQube server running and configured
- Nexus Repository Manager running
- DockerHub account
- Terraform >= 1.0

---

## 🚦 How to Run

### 1. Provision Infrastructure
```bash
cd bankapp-terraform
terraform init
terraform plan
terraform apply
```

### 2. Configure Jenkins
- Add credentials: `github-deepak`, `docker-cred`, `k8-token`, `sonar-token`
- Install required plugins
- Configure SonarQube and Maven tools in Global Tool Configuration

### 3. Run CI Pipeline
- Create Jenkins pipeline job pointing to `bankapp-ci/Jenkinsfile`
- Trigger build — pipeline runs all stages automatically

### 4. Run CD Pipeline
- Create Jenkins pipeline job pointing to `bankapp-cd/Jenkinsfile`
- Trigger after CI completes — deploys to EKS

---

## 📬 Notifications

Pipeline sends email notification to `deepakgholave11@gmail.com` on every build with:
- Build number
- Pipeline status (SUCCESS / FAILURE)

---

## 👨‍💻 Author

**Deepak Gholave**  
Senior Systems Engineer | AWS DevOps  
[GitHub](https://github.com/deepakgholave11) | [LinkedIn](https://linkedin.com/in/deepakgholave)

---

> ⭐ If this project helped you, consider giving it a star!
