# 🛠 Kubernetes Troubleshooting & Debugging

When things go wrong, knowing how to quickly identify and fix issues is critical.

---

## 🔍 Common Issues

| Symptom                 | Possible Cause                     | Commands to Investigate                          |
|-------------------------|----------------------------------|-------------------------------------------------|
| Pod CrashLoopBackOff     | App crash / bad config            | `kubectl logs <pod>`<br>`kubectl describe pod`  |
| ImagePullBackOff         | Image not found / registry issues | `kubectl describe pod`                           |
| Node NotReady            | Node down / kubelet issues        | `kubectl get nodes`<br>`kubectl describe node`  |
| PVC Pending              | Storage provisioning failed       | `kubectl describe pvc`                           |

---

## 🧰 Useful `kubectl` Commands

```bash
kubectl get pods --all-namespaces
kubectl describe pod <pod-name>
kubectl logs <pod-name> [-c container-name]
kubectl exec -it <pod-name> -- /bin/bash
kubectl top nodes
kubectl top pods
kubectl get events --sort-by='.metadata.creationTimestamp'

🐞 Debugging Pods
Check pod logs (kubectl logs)
Exec into pod to check environment
Describe pod for events & status
Check resource limits/requests
⚙️ Debugging Nodes
SSH into node
Check kubelet status: systemctl status kubelet
Inspect node logs: journalctl -u kubelet
Verify networking & disk space
🔧 Using kubectl debug (Kubernetes 1.18+)
Create ephemeral debug container in the same namespace:

kubectl debug pod/<pod-name> -it --image=busybox
Debug node issues with:

kubectl debug node/<node-name> -it --image=busybox
📄 Check Cluster Events

kubectl get events --all-namespaces --sort-by='.metadata.creationTimestamp'
Identify recent errors or warnings.

✅ Best Practices
Always check logs and events first
Use resource limits to avoid OOM kills
Monitor cluster metrics continuously
Automate alerts for critical issues
Use namespaces to isolate workloads


