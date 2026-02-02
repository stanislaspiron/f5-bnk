# NFS Storage
## Install NFS requirements
```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
```

Check installation

```
kubectl --namespace=local-path-storage get pods --watch
```

