# 🌐 Kubernetes Networking Overview

Kubernetes handles communication:
- Between Pods (via Pod IPs)
- Between Services and Pods
- Between external clients and Services

---

## 🧩 Key Networking Concepts

### 📍 Pod-to-Pod Communication
Every Pod gets a unique IP. Pods can reach each other directly in the same cluster.

### 🧭 DNS in Kubernetes
KubeDNS/CoreDNS provides service discovery via:
```bash
<service-name>.<namespace>.svc.cluster.local

🌐 Services
Kubernetes Services expose Pods:

ClusterIP (default): Internal access only

NodePort: Accessible via <NodeIP>:<Port>

LoadBalancer: Works with cloud providers

🎯 Example: Create ClusterIP and NodePort Services

See service-types.yaml for examples.

---

## 📄 `04-networking/service-types.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-clusterip
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30036
  type: NodePort

📄 04-networking/ingress-example.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: nginx.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-clusterip
                port:
                  number: 80

🚀 Ingress Controller Setup (Optional)
To use Ingress, install a controller (e.g., NGINX Ingress):
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml

✅ Hands-On Practice
Apply a Deployment for nginx:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80

Apply service types:
kubectl apply -f service-types.yaml

Use NodePort or Ingress to test externally.



