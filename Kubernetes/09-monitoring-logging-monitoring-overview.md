# 📊 Logging & Monitoring in Kubernetes

Monitoring and logging are essential for:
- Observability
- Alerting
- Troubleshooting
- Capacity planning

---

## 📡 Monitoring Stack

✅ Common Open-Source Tools:
- **Prometheus**: Metrics collection
- **Grafana**: Dashboards/visualization
- **Node Exporter**: Node-level metrics
- **kube-state-metrics**: K8s object state
- **Alertmanager**: Prometheus alerts

---

## 📄 Deploy Prometheus + Grafana via Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install k8s-monitoring prometheus-community/kube-prometheus-stack

Access Grafana:

kubectl port-forward svc/k8s-monitoring-grafana 3000:80
# Default user/pass: admin / prom-operator

📈 Metrics Server (for HPA)
Install:

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
Verify:

kubectl top nodes
kubectl top pods

📁 09-monitoring-logging/logging-overview.md

# 📘 Kubernetes Logging Overview

There are 3 logging levels:
- **Node Level** (container logs via `docker` or `containerd`)
- **Cluster Level** (using EFK or Loki stack)
- **Application Level** (custom logs inside containers)

---

## 📦 Common Logging Tools

| Tool       | Purpose                  |
|------------|--------------------------|
| Fluentd    | Log collector/forwarder  |
| Elasticsearch | Log storage & search  |
| Kibana     | UI for logs              |
| Loki       | Lightweight log collector by Grafana |
| Promtail   | Loki's log shipper       |

---

## 📄 EFK Stack (Elasticsearch, Fluentd, Kibana)

Use Helm:
```bash
helm repo add elastic https://helm.elastic.co
helm install elasticsearch elastic/elasticsearch
helm install kibana elastic/kibana
helm install fluentd stable/fluentd-elasticsearch

🧪 Test Logs
kubectl logs <pod-name>
kubectl logs -f <pod-name> -c <container-name>

✅ Best Practices
Centralize all logs using EFK or Loki
Use log rotation in containers
Use alerts for crash loops or node issues
Enable monitoring for:
CPU & Memory
Disk I/O
Pod failures
Latency

🚨 Alerts with Prometheus
groups:
  - name: pod-alerts
    rules:
      - alert: PodCrashLooping
        expr: rate(kube_pod_container_status_restarts_total[5m]) > 0.1
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "Pod is crash looping"




