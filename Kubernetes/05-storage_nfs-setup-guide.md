# 🗂️ Storage in Kubernetes: PV, PVC & NFS

Kubernetes uses **Volumes** for persistent data in Pods.

## 🔖 Volume Types
- `emptyDir`: Temporary storage
- `hostPath`: Host-mounted path
- `nfs`: External NFS server
- `awsEBS`, `gcePD`, `csi`: Cloud volumes
- `persistentVolumeClaim`: Use pre-defined or dynamic storage

---

## 🛠️ Setup NFS Server (Ubuntu)
```bash
sudo apt update
sudo apt install nfs-kernel-server -y
sudo mkdir -p /srv/nfs/kubedata
sudo chown nobody:nogroup /srv/nfs/kubedata
sudo chmod 777 /srv/nfs/kubedata

Edit exports:
sudo nano /etc/exports
/srv/nfs/kubedata *(rw,sync,no_subtree_check,no_root_squash)
Apply config:

sudo exportfs -rav
sudo systemctl restart nfs-kernel-server

📍 On Worker/Client Nodes
sudo apt install nfs-common -y

📄 05-storage/pv-pvc-example.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    path: /srv/nfs/kubedata
    server: 192.168.1.100  # Replace with your NFS server IP
  persistentVolumeReclaimPolicy: Retain
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 500Mi

📘 Use PVC in Pod

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pv-pod
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: nfs-volume
  volumes:
    - name: nfs-volume
      persistentVolumeClaim:
        claimName: nfs-pvc

✅ Commands to Try

kubectl apply -f pv-pvc-example.yaml
kubectl get pv,pvc
kubectl describe pvc nfs-pvc

📌 Pro Tips

Use ReadWriteMany for shared storage (NFS, GlusterFS).
Use persistentVolumeReclaimPolicy: Retain to avoid auto-deletion.
For dynamic provisioning, install a CSI driver (like NFS-CSI or hostpath-provisioner).


