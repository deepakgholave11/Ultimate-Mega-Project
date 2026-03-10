# 🏦 BankApp — Production-Style DevOps Pipeline on AWS EKS

![CI](https://img.shields.io/badge/CI-Jenkins-blue?logo=jenkins) ![CD](https://img.shields.io/badge/CD-ArgoCD-orange?logo=argo) ![Cloud](https://img.shields.io/badge/Cloud-AWS%20EKS-yellow?logo=amazonaws) ![IaC](https://img.shields.io/badge/IaC-Terraform-purple?logo=terraform) ![Security](https://img.shields.io/badge/Security-Trivy%20%7C%20SonarQube-green)

A complete end-to-end DevOps implementation for a Java-based Banking Application — covering CI/CD automation, containerization, Kubernetes orchestration, infrastructure provisioning, and security scanning.

---

## 📐 Architecture Overview
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BankApp DevOps Architecture</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Syne:wght@700;800&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --border: #1e2d45;
    --accent1: #00d4ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --accent4: #f59e0b;
    --accent5: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
  }

  body {
    background: var(--bg);
    font-family: 'JetBrains Mono', monospace;
    color: var(--text);
    min-height: 100vh;
    padding: 40px 20px;
    background-image: 
      radial-gradient(ellipse at 20% 20%, rgba(0,212,255,0.04) 0%, transparent 50%),
      radial-gradient(ellipse at 80% 80%, rgba(124,58,237,0.04) 0%, transparent 50%);
  }

  h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(1.4rem, 3vw, 2.2rem);
    font-weight: 800;
    text-align: center;
    margin-bottom: 8px;
    background: linear-gradient(135deg, var(--accent1), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -0.5px;
  }

  .subtitle {
    text-align: center;
    color: var(--muted);
    font-size: 0.75rem;
    margin-bottom: 48px;
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  .diagram {
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 0;
    align-items: center;
  }

  /* Row layout */
  .row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0;
    width: 100%;
  }

  /* Node box */
  .node {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 14px 18px;
    min-width: 130px;
    text-align: center;
    position: relative;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .node:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(0,212,255,0.1);
  }
  .node .icon { font-size: 1.5rem; display: block; margin-bottom: 6px; }
  .node .label { font-size: 0.7rem; font-weight: 600; color: var(--text); line-height: 1.3; }
  .node .sub { font-size: 0.6rem; color: var(--muted); margin-top: 3px; }

  /* Color variants */
  .node.blue   { border-color: rgba(0,212,255,0.4);   box-shadow: 0 0 20px rgba(0,212,255,0.06); }
  .node.purple { border-color: rgba(124,58,237,0.4);  box-shadow: 0 0 20px rgba(124,58,237,0.06); }
  .node.green  { border-color: rgba(16,185,129,0.4);  box-shadow: 0 0 20px rgba(16,185,129,0.06); }
  .node.amber  { border-color: rgba(245,158,11,0.4);  box-shadow: 0 0 20px rgba(245,158,11,0.06); }
  .node.red    { border-color: rgba(239,68,68,0.4);   box-shadow: 0 0 20px rgba(239,68,68,0.06); }

  /* Big pipeline box */
  .pipeline-box {
    border-radius: 16px;
    padding: 20px 24px;
    position: relative;
  }
  .pipeline-box .pipe-title {
    font-family: 'Syne', sans-serif;
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 16px;
    text-align: center;
  }
  .pipeline-box.ci {
    background: linear-gradient(135deg, rgba(0,212,255,0.05), rgba(124,58,237,0.05));
    border: 1px solid rgba(0,212,255,0.2);
    width: 100%;
    max-width: 900px;
  }
  .pipeline-box.ci .pipe-title { color: var(--accent1); }
  .pipeline-box.cd {
    background: linear-gradient(135deg, rgba(16,185,129,0.05), rgba(245,158,11,0.05));
    border: 1px solid rgba(16,185,129,0.2);
    width: 100%;
    max-width: 900px;
  }
  .pipeline-box.cd .pipe-title { color: var(--accent3); }

  /* Stages inside pipeline */
  .stages {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    align-items: center;
  }

  .stage {
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 8px;
    padding: 8px 12px;
    text-align: center;
    min-width: 90px;
  }
  .stage .s-icon { font-size: 1.1rem; display: block; margin-bottom: 4px; }
  .stage .s-label { font-size: 0.58rem; color: var(--text); font-weight: 600; }
  .stage .s-tool { font-size: 0.52rem; color: var(--muted); margin-top: 2px; }

  /* Arrows */
  .arrow-down {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 36px;
    position: relative;
  }
  .arrow-down::before {
    content: '';
    width: 2px;
    height: 24px;
    background: linear-gradient(to bottom, var(--accent1), var(--accent2));
    display: block;
  }
  .arrow-down::after {
    content: '▼';
    color: var(--accent2);
    font-size: 0.7rem;
    margin-top: -2px;
  }

  .arrow-right {
    width: 32px;
    height: 2px;
    background: linear-gradient(to right, var(--accent1), var(--accent2));
    position: relative;
    flex-shrink: 0;
  }
  .arrow-right::after {
    content: '▶';
    color: var(--accent2);
    font-size: 0.6rem;
    position: absolute;
    right: -6px;
    top: -6px;
  }

  .stage-arrow {
    color: var(--muted);
    font-size: 0.7rem;
    flex-shrink: 0;
    padding: 0 2px;
  }

  /* EKS cluster */
  .eks-box {
    background: linear-gradient(135deg, rgba(245,158,11,0.06), rgba(239,68,68,0.06));
    border: 1px solid rgba(245,158,11,0.3);
    border-radius: 16px;
    padding: 20px 30px;
    text-align: center;
    width: 100%;
    max-width: 900px;
  }
  .eks-box .eks-title {
    font-family: 'Syne', sans-serif;
    color: var(--accent4);
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 16px;
  }
  .eks-inner {
    display: flex;
    justify-content: center;
    gap: 16px;
    flex-wrap: wrap;
  }
  .k8s-resource {
    background: rgba(255,255,255,0.04);
    border: 1px dashed rgba(245,158,11,0.3);
    border-radius: 10px;
    padding: 10px 16px;
    text-align: center;
    min-width: 100px;
  }
  .k8s-resource .r-icon { font-size: 1.2rem; display: block; margin-bottom: 4px; }
  .k8s-resource .r-label { font-size: 0.6rem; font-weight: 600; color: var(--text); }
  .k8s-resource .r-sub { font-size: 0.55rem; color: var(--muted); margin-top: 2px; }

  /* Supporting tools row */
  .tools-row {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
    max-width: 900px;
  }

  .tool-chip {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 6px 14px;
    font-size: 0.62rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 6px;
    color: var(--muted);
  }
  .tool-chip span { font-size: 0.9rem; }

  /* Legend */
  .legend {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 32px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
    max-width: 900px;
    width: 100%;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.6rem;
    color: var(--muted);
  }
  .legend-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    flex-shrink: 0;
  }

  .gitops-label {
    font-size: 0.58rem;
    color: var(--accent3);
    text-align: center;
    margin: 2px 0;
    letter-spacing: 1px;
    text-transform: uppercase;
  }
</style>
</head>
<body>

<h1>🏦 BankApp — DevOps Architecture</h1>
<p class="subtitle">CI/CD · Kubernetes · AWS EKS · Terraform · Security</p>

<div class="diagram">

  <!-- Row 1: Developer + GitHub -->
  <div class="row" style="gap:0; justify-content:flex-start; max-width:900px; width:100%;">
    <div class="node blue">
      <span class="icon">👨‍💻</span>
      <div class="label">Developer</div>
      <div class="sub">git push</div>
    </div>
    <div class="arrow-right"></div>
    <div class="node blue" style="margin-left:8px;">
      <span class="icon">🐙</span>
      <div class="label">GitHub</div>
      <div class="sub">Source Repo</div>
    </div>
  </div>

  <div class="arrow-down"></div>

  <!-- CI Pipeline -->
  <div class="pipeline-box ci">
    <div class="pipe-title">⚙️ Jenkins CI Pipeline</div>
    <div class="stages">
      <div class="stage">
        <span class="s-icon">📥</span>
        <div class="s-label">Git Checkout</div>
        <div class="s-tool">Jenkins</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">⚒️</span>
        <div class="s-label">Compile + Test</div>
        <div class="s-tool">Maven 3</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">🔍</span>
        <div class="s-label">Trivy FS Scan</div>
        <div class="s-tool">Trivy</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">📊</span>
        <div class="s-label">SonarQube</div>
        <div class="s-tool">Quality Gate</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">📦</span>
        <div class="s-label">Publish</div>
        <div class="s-tool">Nexus</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">🐳</span>
        <div class="s-label">Docker Build</div>
        <div class="s-tool">v${BUILD_NO}</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">🛡️</span>
        <div class="s-label">Image Scan</div>
        <div class="s-tool">Trivy</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">☁️</span>
        <div class="s-label">Docker Push</div>
        <div class="s-tool">DockerHub</div>
      </div>
    </div>
  </div>

  <div class="arrow-down"></div>
  <p class="gitops-label">🔄 GitOps — updates manifest.yaml image tag</p>

  <!-- GitHub CD Repo -->
  <div class="node green" style="margin: 4px 0;">
    <span class="icon">🐙</span>
    <div class="label">GitHub</div>
    <div class="sub">CD Manifests Repo</div>
  </div>

  <div class="arrow-down"></div>

  <!-- CD Pipeline -->
  <div class="pipeline-box cd">
    <div class="pipe-title">🚀 Jenkins CD Pipeline</div>
    <div class="stages">
      <div class="stage">
        <span class="s-icon">📥</span>
        <div class="s-label">Checkout CD</div>
        <div class="s-tool">Jenkins</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">☸️</span>
        <div class="s-label">kubectl apply</div>
        <div class="s-tool">manifest.yaml</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">📈</span>
        <div class="s-label">Apply HPA</div>
        <div class="s-tool">HPA.yaml</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">✅</span>
        <div class="s-label">Verify Pods</div>
        <div class="s-tool">kubectl get</div>
      </div>
      <div class="stage-arrow">→</div>
      <div class="stage">
        <span class="s-icon">📧</span>
        <div class="s-label">Notify</div>
        <div class="s-tool">Email</div>
      </div>
    </div>
  </div>

  <div class="arrow-down"></div>

  <!-- EKS Cluster -->
  <div class="eks-box">
    <div class="eks-title">☁️ AWS EKS Cluster — ap-south-1 | Namespace: webapps</div>
    <div class="eks-inner">
      <div class="k8s-resource">
        <span class="r-icon">🟦</span>
        <div class="r-label">Deployment</div>
        <div class="r-sub">BankApp Pods</div>
      </div>
      <div class="k8s-resource">
        <span class="r-icon">🌐</span>
        <div class="r-label">Service</div>
        <div class="r-sub">LoadBalancer</div>
      </div>
      <div class="k8s-resource">
        <span class="r-icon">📈</span>
        <div class="r-label">HPA</div>
        <div class="r-sub">Auto Scaling</div>
      </div>
      <div class="k8s-resource">
        <span class="r-icon">🔐</span>
        <div class="r-label">RBAC</div>
        <div class="r-sub">k8s-token</div>
      </div>
    </div>
  </div>

  <div class="arrow-down"></div>

  <!-- Terraform -->
  <div class="node purple">
    <span class="icon">🏗️</span>
    <div class="label">Terraform</div>
    <div class="sub">VPC · EKS · EC2 · IAM</div>
  </div>

  <!-- Supporting Tools -->
  <div style="margin-top: 40px; width:100%; max-width:900px; text-align:center;">
    <p style="font-size:0.62rem; color:var(--muted); text-transform:uppercase; letter-spacing:2px; margin-bottom:12px;">Supporting Infrastructure</p>
    <div class="tools-row">
      <div class="tool-chip"><span>🔵</span> Jenkins Server (EC2)</div>
      <div class="tool-chip"><span>🟠</span> SonarQube (EC2)</div>
      <div class="tool-chip"><span>🟤</span> Nexus Repository (EC2)</div>
      <div class="tool-chip"><span>🐳</span> DockerHub Registry</div>
      <div class="tool-chip"><span>🛡️</span> Trivy Scanner</div>
      <div class="tool-chip"><span>📧</span> Email Notifications</div>
    </div>
  </div>

  <!-- Legend -->
  <div class="legend">
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent1)"></div> CI Flow</div>
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent3)"></div> GitOps / CD Flow</div>
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent4)"></div> AWS Infrastructure</div>
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent2)"></div> IaC / Terraform</div>
    <div class="legend-item"><div class="legend-dot" style="background:var(--accent5)"></div> Security Scanning</div>
  </div>

</div>

</body>
</html>
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
