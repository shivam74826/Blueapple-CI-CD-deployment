# ⚙️ kubectl Commands & Core Resources

## 🧩 What is kubectl?
`kubectl` is the command-line tool used to interact with Kubernetes clusters.

---

### 🔧 Basic Commands

```bash
kubectl version
kubectl cluster-info
kubectl get nodes
kubectl get pods
kubectl describe pod <pod-name>

🧱 Create Objects from YAML
kubectl apply -f pod.yaml
kubectl create -f deployment.yaml
kubectl delete -f pod.yaml

🧹 Namespace Usage
kubectl create namespace dev
kubectl get all -n dev
kubectl config set-context --current --namespace=dev

📦 Core Resources

| Resource   | Description                   |
| ---------- | ----------------------------- |
| Pod        | Basic unit with containers    |
| Deployment | Manages pods + replicas       |
| ReplicaSet | Ensures pod replica count     |
| Service    | Exposes apps (NodePort, etc.) |
| Namespace  | Multi-tenant isolation        |
| ConfigMap  | Inject config into pods       |
| Secret     | Inject sensitive data         |

⛏️ Useful Tips
Always use --dry-run=client -o yaml to preview changes.
Use kubectl explain <resource> to understand structure.

---

## 📄 `02-kubectl-core-resources/pod-deployment.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80

02-kubectl-core-resources/namespace-config.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-environment

✅ Hands-on Practice
Apply namespace:

kubectl apply -f namespace-config.yaml
Set current namespace:

kubectl config set-context --current --namespace=dev-environment
Deploy the nginx pod:

kubectl apply -f pod-deployment.yaml
kubectl get pods


