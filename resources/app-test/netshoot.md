# Create a pod to ping resources in ${app_namespace}

## Variables

Choose one option in:
- workload
- f5-tmm

### on workload2
```
app_namespace="myapp2"
node_app="workload2"
```

### on f5-tmm

```
app_namespace="myapp"
node_app="f5-tmm"
```

## Create namespace if not exists

```bash
kubectl create namespace ${app_namespace}
```
## Deploy netshoot pod
```bash
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: netshoot
  namespace: ${app_namespace}
  labels:
    app: netshoot
spec:
  nodeSelector:
    app: ${node_app}
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
kubectl exec -it -n ${app_namespace} netshoot -- /bin/bash
```
