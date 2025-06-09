# 🎮 Kubernetes Controllers Overview

Controllers automate workload management in Kubernetes. They ensure desired state matches the current state.

---

## 🔧 Types of Controllers

| Controller     | Purpose                                          |
|----------------|--------------------------------------------------|
| Deployment     | Manages stateless apps (rolling updates, replicas) |
| StatefulSet    | Manages stateful apps (with identity & storage) |
| DaemonSet      | Runs a pod on every node                        |
| Job            | Runs a task once                                |
| CronJob        | Schedules Jobs using cron expressions           |

---

## ✅ Controller Basics

```bash
kubectl get deployments
kubectl get statefulsets
kubectl get daemonsets
kubectl get cronjobs

Pro Tip:
Use kubectl rollout status deployment/<name> to monitor deployment progress.
---

## 📄 `06-controllers/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: nginx
          ports:
            - containerPort: 80

📄 06-controllers/statefulset.yaml

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi

📄 06-controllers/daemonset.yaml

apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
spec:
  selector:
    matchLabels:
      name: node-exporter
  template:
    metadata:
      labels:
        name: node-exporter
    spec:
      containers:
        - name: node-exporter
          image: prom/node-exporter

📄 06-controllers/cronjob.yaml

apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cron
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: busybox
              args:
                - /bin/sh
                - -c
                - date; echo Hello from Kubernetes CronJob!
          restartPolicy: OnFailure


✅ Best Practices

Use Deployments for most workloads.
Use StatefulSets for DBs and apps needing sticky identity.
Use DaemonSets for monitoring/logging/agents.
Monitor Jobs and CronJobs for completion and logs.


