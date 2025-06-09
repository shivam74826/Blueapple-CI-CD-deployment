
# 🔐 Kubernetes Security Essentials

Security in Kubernetes includes:

- Authentication (who are you?)
- Authorization (what can you do?)
- Admission (what’s allowed to run?)
- Network & Runtime Security

---

## 🔑 1. Authentication Methods

| Method       | Use Case                |
|--------------|-------------------------|
| X.509 certs  | kubectl auth            |
| Token        | ServiceAccount access   |
| OIDC         | External auth providers |

Check current user:
```bash
kubectl config view --minify

✅ 2. RBAC (Role-Based Access Control)
Roles define what actions are allowed.

🔧 Role vs ClusterRole
Role: Namespaced
ClusterRole: Cluster-wide

📄 08-security/rbac-example.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dev-user
  namespace: dev
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev
subjects:
  - kind: ServiceAccount
    name: dev-user
    namespace: dev
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

🧱 3. Network Policies
Restrict pod-level communication (like a firewall).

📄 08-security/network-policy.yaml

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: default
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
🛡️ This blocks all traffic by default. You then allow traffic explicitly.

🔐 4. Secrets Management
kubectl create secret generic db-creds --from-literal=user=admin --from-literal=password=secret

env:
  - name: DB_USER
    valueFrom:
      secretKeyRef:
        name: db-creds
        key: user

🧾 5. Admission Controllers
Built-in or external policies to validate/modify requests.

Examples:
NamespaceLifecycle
PodSecurity
LimitRanger

🛡️ Security Best Practices
Never run containers as root (use securityContext)
Use RBAC + NetworkPolicies
Rotate credentials and secrets
Scan images for vulnerabilities
Enable audit logging and pod security standards



