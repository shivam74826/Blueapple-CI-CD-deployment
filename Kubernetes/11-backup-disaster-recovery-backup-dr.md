# 💾 Backup & Disaster Recovery in Kubernetes

Clusters can fail. Data loss is unacceptable. Plan backups and recovery carefully.

---

## 🔑 Key Backup Targets

- ETCD (Kubernetes state)
- Persistent Volumes (PV)
- Application Data

---

## 1. ETCD Backup

### Backup ETCD (for clusters using etcd directly):

```bash
ETCDCTL_API=3 etcdctl snapshot save snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key

Restore ETCD:
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
  --data-dir /var/lib/etcd-from-backup

2. Persistent Volume Backup
Use Velero for backing up PVs and cluster resources.

Supports AWS, Azure, GCP, and on-premise solutions.

3. Velero Installation & Usage
Install Velero CLI:
curl -LO https://github.com/vmware-tanzu/velero/releases/download/v1.12.0/velero-v1.12.0-linux-amd64.tar.gz
tar -xvf velero-v1.12.0-linux-amd64.tar.gz
sudo mv velero-v1.12.0-linux-amd64/velero /usr/local/bin/

Install Velero Server on cluster (example with AWS S3):
velero install \
  --provider aws \
  --bucket my-bucket \
  --secret-file ./credentials-velero \
  --backup-location-config region=us-east-1 \
  --snapshot-location-config region=us-east-1

Create backup:
velero backup create cluster-backup

Restore backup:
velero restore create --from-backup cluster-backup

4. Application-level Backups
Use DB-native tools (e.g., pg_dump for PostgreSQL)

Export/import application configs as YAML

✅ Best Practices
Schedule regular backups with cron jobs or Velero schedules
Test restores frequently in staging
Keep backups encrypted and offsite
Automate backup alerts and monitoring



