# NFS Storage
## Install NFS requirements
```
helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts
helm install csi-driver-nfs csi-driver-nfs/csi-driver-nfs --namespace kube-system --set kubeletDir=/var/lib/kubelet
```

Check installation

```
kubectl --namespace=kube-system get pods --selector="app.kubernetes.io/instance=csi-driver-nfs" --watch
```
## Create NFS Storage class

Before Creating storage class, the NFS server must be deployed. short guide [here](./nfs-server.md)

```
cat <<EOF | kubectl apply -f -
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: nfs
provisioner: nfs.csi.k8s.io
parameters:
  server: ${nfs_server}
  share: ${nfs_path}
reclaimPolicy: Retain
volumeBindingMode: Immediate
EOF
```

## Define NFS as default storage Class
```
kubectl patch storageclass nfs -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
