# 🎓 Module 15: Final Preparation for CKA Exam

This module guides you through:
- Understanding the exam structure
- Preparing practical skills
- Setting up the right environment
- Mastering time & resource management

---

## 🧪 CKA Exam Overview

| Feature               | Details                             |
|-----------------------|-------------------------------------|
| Duration              | 2 hours                             |
| Type                  | Hands-on, open book (no internet)   |
| Passing Score         | 66%                                 |
| Total Questions       | ~15–20 tasks                        |
| Allowed Resources     | [kubernetes.io](https://kubernetes.io), GitHub repos |
| Platform              | Remote proctored exam via PSI       |

---

## 🗂️ CKA Domains & Weights

| Domain                        | Weight |
|-------------------------------|--------|
| Cluster Architecture & Installation | 10%   |
| Workloads & Scheduling         | 15%   |
| Services & Networking          | 20%   |
| Storage                        | 10%   |
| Troubleshooting                | 30%   |
| Security, Monitoring & Logging| 15%   |

---

## 🧰 Exam Setup Tips

### 🔧 Use Alias File (`.bashrc` or `.bash_aliases`)
```bash
alias k='kubectl'
alias kgp='kubectl get pods'
alias kgn='kubectl get nodes'
alias kaf='kubectl apply -f'
alias kd='kubectl describe'
alias kl='kubectl logs'

📂 Directory & YAML Tips

Save all your YAML files to /home/candidate/
Use kubectl explain for quick schema help
Use --dry-run=client -o yaml to scaffold objects

✅ Exam Practice Checklist
 Deploy & upgrade a Kubernetes cluster (kubeadm)
 Create and troubleshoot pods, deployments, jobs
 Work with Services: ClusterIP, NodePort, LoadBalancer
 Manage ConfigMaps & Secrets
 PersistentVolume + PVC setup & debug
 NetworkPolicy and security context
 Troubleshoot CrashLoopBackOff, networking, DNS
 Create backup of etcd and restore it
 Debug node issues using journalctl, systemctl

🎯 Tips to Maximize Score
Tip	Benefit
Use k -n <namespace> consistently	Avoid wrong namespace errors
Label important questions	Review them later
Use kubectl explain	Quick API reference
Practice 20+ real scenarios	Build muscle memory
Backup YAML manifests quickly	Save working solutions to re-use

