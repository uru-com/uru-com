# 🚀 Kubernetes Cluster Creation with Kops on AWS

![Kops Architecture](../images/kops-cluster-flow.png)

## 📖 Overview
This guide documents an end-to-end Kubernetes cluster setup on AWS using Kops.

Key steps:

1. Prepare the AWS and local CLI environment
2. Configure the S3 state store
3. Create and review the cluster spec
4. Deploy the cluster with Kops
5. Validate with kubectl
6. Deploy an application and expose it with a LoadBalancer

---

## 🏗️ Architecture Diagrams

### End-to-End Kops Flow
![Kops Flow](../images/kops-cluster-flow.png)

### Cluster Creation Flow
![Cluster Creation Flow](../images/cluster-creation-flow.png)

### Kops vs kubectl
![Kops vs kubectl](../images/kubectl-vs-kops.png)

### Practice vs Production Architecture
![Practice vs Production](../images/practice-vs-production.png)

---

## ⚙️ Prerequisites

| Requirement | Description |
|---|---|
| AWS Account | Active AWS account with proper permissions |
| Domain | Route53-compatible domain |
| AWS CLI | Installed and configured |
| kubectl | Installed Kubernetes CLI |
| kops | Installed Kubernetes operations tool |
| SSH Key | `~/.ssh/id_ed25519.pub` available |

---

## 🔧 Installation

### Install AWS CLI

```bash
brew install awscli
aws configure
```

### Install kubectl

```bash
brew install kubectl
```

### Install Kops

```bash
brew install kops
```

---

## 🪣 Create S3 State Store

```bash
aws s3 mb s3://urukundu-kops-state-store
```

```bash
aws s3api put-bucket-versioning \
  --bucket urukundu-kops-state-store \
  --versioning-configuration Status=Enabled
```

```bash
export KOPS_STATE_STORE=s3://urukundu-kops-state-store
```

---

## 🌐 Create Route53 Hosted Zone

```bash
aws route53 create-hosted-zone \
  --name urukundu.com \
  --caller-reference $(date +%s)
```

Verify DNS delegation:

```bash
dig NS urukundu.com
```

---

## ☸️ Create Kubernetes Cluster

```bash
kops create cluster \
  --name k8s.urukundu.com \
  --state s3://urukundu-kops-state-store \
  --zones us-east-1a \
  --node-count 2 \
  --node-size t3.small \
  --control-plane-count 1 \
  --control-plane-size t3.small \
  --ssh-public-key ~/.ssh/id_ed25519.pub
```

Apply the cluster config:

```bash
kops update cluster \
  --name k8s.urukundu.com \
  --yes \
  --admin
```

---

## ✅ Validation

```bash
kops validate cluster
```

```bash
kubectl get nodes
```

```bash
kubectl get pods -A
```

---

## 🚀 Deploy Nginx

```bash
kubectl create deployment nginx --image=nginx
```

```bash
kubectl expose deployment nginx \
  --port=80 \
  --type=LoadBalancer
```

```bash
kubectl get svc
```

---

## 🧹 Cleanup

```bash
kubectl delete svc nginx
kubectl delete deployment nginx
```

```bash
kops delete cluster \
  --name k8s.urukundu.com \
  --yes
```

---

## 📚 Learning Outcomes

- Kubernetes fundamentals
- Kops administration
- AWS networking and Route53 DNS
- S3 state management
- IAM and instance profiles
- Load balancers and services
- Kubernetes deployments and validation
