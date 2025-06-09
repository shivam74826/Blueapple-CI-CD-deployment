# 🌐 Module 14: Service Mesh Concepts (Istio / Linkerd)

A **Service Mesh** is a dedicated infrastructure layer for:
- Traffic management
- Security (mTLS)
- Observability
- Resilience (Retries, Timeouts, Circuit Breaking)

---

## 🤔 Why Use a Service Mesh?

| Feature              | Benefits                                     |
|----------------------|----------------------------------------------|
| Secure communication | mTLS between services                        |
| Traffic routing      | Canary, blue-green, A/B testing              |
| Fault tolerance      | Retries, timeouts, circuit breaking          |
| Observability        | Built-in metrics, tracing, and logging       |
| Policy enforcement   | Access controls, rate limiting               |

---

## 🧱 Popular Service Mesh Tools

| Tool     | Description                           |
|----------|---------------------------------------|
| Istio    | Feature-rich, widely used, complex    |
| Linkerd  | Lightweight, easy to set up           |
| Consul   | Integrated with HashiCorp tools       |
| Kuma     | Built on Envoy proxy, by Kong         |

---

## 🚀 Installing Istio (via Istioctl)

### Download and Install Istio:

```bash
curl -L https://istio.io/downloadIstio | sh -
cd istio-*/bin
export PATH=$PWD:$PATH

Install Istio core:
istioctl install --set profile=demo -y
kubectl label namespace default istio-injection=enabled

🌍 Istio Core Components
Component	Role
Envoy Proxy	Sidecar proxy for each pod
Pilot	Service discovery & traffic
Mixer	Telemetry & policy (deprecated)
Citadel	Certificate management (mTLS)
Galley	Configuration validation

📊 Observability in Istio

Prometheus – Metrics
Grafana – Visualization
Kiali – Service map
Jaeger – Tracing
Access dashboard:
istioctl dashboard kiali

📈 Traffic Management
Example VirtualService (Canary):

apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: myapp
spec:
  hosts:
  - myapp.default.svc.cluster.local
  http:
  - route:
    - destination:
        host: myapp
        subset: v1
      weight: 80
    - destination:
        host: myapp
        subset: v2
      weight: 20

🔐 Security Features
mTLS: Pod-to-pod encryption
Authorization Policies: Control who can access what
Rate Limiting & JWT Auth: Apply API security

✅ Best Practices
Start with demo profile and expand gradually
Use Kiali for monitoring service mesh traffic
Apply strict mTLS in production
Separate gateway and app namespaces
Use Prometheus + Grafana + Jaeger

🧪 Quick Test
Inject sidecar into a pod manually:
istioctl kube-inject -f deployment.yaml | kubectl apply -f -
