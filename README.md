# 🚀 Strapi Deployment on ECS Fargate with Terraform + GitHub Actions

---

# 📌 Deployment Strategy Rule

ALWAYS make it work manually first → then automate via Terraform + GitHub Actions

### Why?

- Easier debugging
- Better AWS understanding
- CI/CD issues easier to fix

---

# 🧭 Big Picture Architecture

```
Code Push → GitHub Actions
        → Build Docker Image
        → Tag Image
        → Push to ECR
        → Terraform Apply
        → Update ECS Task Definition
        → Fargate Deploys Container
```

---

# 🧱 5 Major Layers

1. Strapi App (Containerized)
2. ECR (Image Registry)
3. ECS Cluster
4. Fargate Service
5. GitHub Actions Automation

---

# ⚙️ Step 0 — Prerequisites

- AWS Account
- IAM User (ECS, ECR, VPC, IAM, CloudWatch)
- AWS CLI configured

Test:

```bash
aws sts get-caller-identity
```

---

# 🚀 Phase‑2 — Push Image to ECR (Manual)

## Create Repo

```bash
aws ecr create-repository   --repository-name strapi-app   --region us-east-1
```

## Login

```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com
```

## Tag

```bash
docker tag strapi-prod:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/strapi-app:v1
```

## Push

```bash
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/strapi-app:v1
```

Verify in ECR console.

---

# 🏗️ Phase‑3 — Terraform Infrastructure

Infra stack:

```
VPC
Subnets
Security Groups
ECR
ECS Cluster
IAM Roles
Task Definition
Fargate Service
```

---

# 📂 Terraform Structure

```
terraform/
├── main.tf
├── provider.tf
├── variables.tf
├── outputs.tf
└── modules/
    ├── vpc/
    ├── ecs/
    ├── ecr/
    ├── iam/
    └── service/
```

---

# 🚀 Phase‑4 — Deploy Container

Add modules:

```
modules/
 ├── task-definition/
 └── service/
```

Responsibilities:

- Pull image
- Run container
- Expose port 1337
- Launch on Fargate

---

# 🔍 Verify Deployment

```
ECS → Cluster → Services → Tasks
```

Status:

```
RUNNING 1/1
```

---

# 🌐 Access App

```
http://<public-ip>:1337/admin
```

---

# 🤖 Phase‑5 — GitHub Actions Automation

Pipeline:

```
Code Push
   ↓
Build Image
   ↓
Tag
   ↓
Push to ECR
   ↓
Update Task Definition
   ↓
Deploy on Fargate
```

---

# 🔄 Responsibility Split

```
Terraform → Infra provisioning
GitHub Actions → App deployments
```

---

# 🧠 Why Not Terraform Every Push?

- Slow pipelines
- Drift risks
- Accidental infra deletion
- Cost impact

---

# ✅ Final Outcome

- Dockerized Strapi
- Image in ECR
- ECS cluster ready
- Fargate runtime
- Automated CI/CD

Production pipeline complete 🚀