# 🎯 Taints and Tolerations

## 🧠 What are Taints?
Taints allow a node to repel pods unless the pod tolerates the taint.

```bash
kubectl taint nodes <node-name> key=value:NoSchedule

✨ Example Use Case:
Taint a node to only allow specific workloads:

kubectl taint nodes node1 app=secure:NoSchedule
✅ Toleration YAML Example:
yaml

tolerations:
  - key: "app"
    operator: "Equal"
    value: "secure"
    effect: "NoSchedule"

This pod can now be scheduled on the tainted node.

---

## 📄 `03-scheduling/node-affinity.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: zone
                operator: In
                values:
                  - zone-a
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80

📘 Notes on Affinity and Anti-Affinity
Node Affinity: Schedule pods on nodes with specific labels.

Pod Affinity: Schedule pods close to others.

Pod Anti-Affinity: Schedule pods away from certain others.

Example to Label Node:
kubectl label nodes <node-name> zone=zone-a
Then apply the pod YAML:

kubectl apply -f node-affinity.yaml
