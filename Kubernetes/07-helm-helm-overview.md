# 📦 Helm: Kubernetes Package Manager

Helm is the "apt" or "yum" of Kubernetes. It simplifies deployment by packaging resources into reusable templates.

---

## 🧩 Helm Concepts

| Term         | Description                                     |
|--------------|-------------------------------------------------|
| Chart        | A Helm package containing Kubernetes manifests  |
| Release      | A chart instance deployed to a cluster          |
| Repository   | Collection of charts (like DockerHub for Helm)  |
| Template     | Parameterized YAML with Go templating           |

---

## 🚀 Install Helm (Linux/macOS)

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

✅ Helm Commands

helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx
helm install my-nginx bitnami/nginx
helm uninstall my-nginx

🏗️ Create Your Own Chart

helm create mychart

Structure:

mychart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── _helpers.tpl

🎛️ Customize Installation
Edit values.yaml to override default values:

replicaCount: 2
image:
  repository: nginx
  tag: stable
  pullPolicy: IfNotPresent

Install with custom values:

helm install myweb ./mychart -f values.yaml
📦 Upgrade & Rollback

helm upgrade myweb ./mychart
helm rollback myweb 1
🎯 Real Use Cases
Managing Prometheus, Grafana, Jenkins

Creating reusable internal charts for microservices

Automating environment deployments

🛡️ Best Practices
Keep values.yaml clean; override in separate env-specific files (e.g., dev-values.yaml)

Use _helpers.tpl for naming conventions and label templates

Version your charts using Chart.yaml




