# certificate manager

## Install certificate manager
```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.19.1/cert-manager.yaml
```
[source](https://cert-manager.io/docs/installation/)

## Check pod creation
```bash
kubectl get pod -n cert-manager --watch
```


[Back](04-far.md)  
[Next](06-certificate-autority.md)