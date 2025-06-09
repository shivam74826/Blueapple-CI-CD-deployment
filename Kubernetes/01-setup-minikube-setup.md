# 🚀 Minikube Setup (For Local Kubernetes Lab)

Minikube runs a single-node Kubernetes cluster inside a VM or container on your laptop.

### ✅ Prerequisites
- VirtualBox or Docker installed
- kubectl installed
- 2 GB RAM minimum

### 🔧 Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

🚀 Start Cluster
minikube start --driver=docker
minikube status
kubectl get nodes

🛠️ Useful Commands
minikube dashboard        # GUI access
minikube stop             # Stop cluster
minikube delete           # Delete cluster


---

## 📁 `01-setup/kubeadm-setup.md`

```md
# 🔧 Setup Kubernetes Cluster Using kubeadm (2 Nodes)

This is a production-like setup using **one master and one worker node**.

### ✅ Prerequisites (Both nodes)
- Ubuntu 20.04+
- 2 CPUs, 2 GB RAM+
- sudo privileges

### Step 1: Disable Swap
```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab

Step 2: Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker

Step 3: Install kubeadm, kubelet, kubectl
sudo apt install -y apt-transport-https ca-certificates curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" |
    sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubelet kubeadm kubectl

Step 4: Initialize Master
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

Step 5: Setup kubectl for User
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

Step 6: Install Pod Network (Calico)
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

Step 7: Join Worker Node (run this on worker)
kubeadm join <MASTER-IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>


---

## 📁 `01-setup/hetzner-setup.md`

```md
# ☁️ Setup Kubernetes Cluster on Hetzner Cloud

Hetzner is a low-cost provider great for self-hosted Kubernetes clusters.

### Prerequisites
- 2+ Hetzner Cloud VMs (Ubuntu 22.04)
- Cloud-init SSH keys ready
- Firewall ports open (6443, 10250, etc.)

### Step 1: Create VMs (1 master + 1 or more workers)

### Step 2: Follow kubeadm steps from previous guide

### Optional: Use Hetzner CSI Driver
```bash
kubectl apply -f https://raw.githubusercontent.com/hetznercloud/csi-driver/main/deploy/kubernetes/hcloud-csi.yml



