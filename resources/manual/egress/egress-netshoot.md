# Create a pod to ping resources in ns1

```bash
kubectl create namespace ns1
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: netshoot
  namespace: ns1
  labels:
    app: netshoot
spec:
  containers:
  - image: nicolaka/netshoot
    command:
      - "sleep"
      - "604800"
    imagePullPolicy: IfNotPresent
    name: netshoot
  restartPolicy: Always
EOF
```

```bash
kubectl exec -it -n ns1 netshoot -- /bin/bash
```


# Create a pod to ping resources in ns2

```bash
kubectl create namespace ns2
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: netshoot
  namespace: ns2
  labels:
    app: netshoot
spec:
#  nodeSelector:
#    app: f5-tmm
  containers:
  - image: nicolaka/netshoot
    command:
      - "sleep"
      - "604800"
    imagePullPolicy: IfNotPresent
    name: netshoot
  restartPolicy: Always
EOF
```

```
kubectl exec -it -n ns2 netshoot -- /bin/bash
```