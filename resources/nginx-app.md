
# Test nginx application

## Variables

Choose one option in:
- workload
- f5-tmm

### on workload2
```
app_namespace="myapp"
node_app="workload2"
```

### on f5-tmm

```
app_namespace="myapp"
node_app="f5-tmm"
```

```bash
kubectl create namespace  ${app_namespace}
```

## Application

```bash
cat << EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: ${app_namespace}
  labels:
    app: nginx-app
spec:
  selector:
    matchLabels:
      app: nginx-app
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx-app
    spec:
      nodeSelector:
        app: ${node_app}
      containers:
      - name: nginx-app
        image: nginx:latest
        ports:
        - name: http
          containerPort: 80
        env:
        - name: DEBIAN_FRONTEND
          value: noninteractive
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: ${app_namespace}
spec:
  selector:
    app: nginx-app
  ports:
  - port: 80
EOF
```

## shell on container

```bash
kubectl exec -it -n ${app_namespace} deployment/nginx-deployment -c nginx-app  -- /bin/bash
```