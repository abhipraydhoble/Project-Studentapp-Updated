
## Create EKS Cluster

#### launch one instance t2.medium

---

### **1: Install eksctl CLI tool for creating EKS Clusters on AWS**

Download eksctl:

```bash
curl --silent --location \
"https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" \
| tar xz -C /tmp
```

Move to binary directory:

```bash
sudo mv /tmp/eksctl /usr/local/bin
```

Check version:

```bash
eksctl version
```

---

### **2: Install kubectl**

Download kubectl:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

Install kubectl:

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

If no root access:

```bash
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```

Check version:

```bash
kubectl version --client
```

---

### **3: Install AWS CLI on Ubuntu**

```bash
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

---

### **4: Configure AWS CLI**

```bash
aws configure
```

---

# ⭐ IMPORTANT CHANGE STARTS HERE

You wanted **Ubuntu nodes**, so Step 5 is rewritten accordingly.

---

# **5: Create Amazon EKS Cluster with UBUNTU Nodes (NOT Amazon Linux)**

### **5.1 Create EKS Control Plane (No Nodes)**

```bash
eksctl create cluster \
  --name my-ekscluster \
  --region ap-southeast-1 \
  --version 1.32 \
  --without-nodegroup
```

---

### **5.2 Get the Ubuntu EKS-Optimized AMI ID**

```bash
aws ssm get-parameters-by-path \
  --path /aws/service/canonical/ubuntu/eks/1.32/optimized-ami/ \
  --region ap-southeast-1
```

Copy the AMI ID (example: `ami-0abc1234d56ef7890`).

---

### **5.3 Create Nodegroup using Ubuntu AMI**

```bash
eksctl create nodegroup \
  --cluster my-ekscluster \
  --region ap-southeast-1 \
  --name ubuntu-nodes \
  --node-type t2.medium \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --node-ami explicit \
  --node-ami-id <YOUR_UBUNTU_AMI_ID>
```

---

### **6: Log In Into EKS cluster**

```bash
aws eks update-kubeconfig --name my-ekscluster
```

---

### **7: Delete EKS Cluster**

```bash
eksctl delete cluster --name my-ekscluster --region ap-southeast-1
```

---


