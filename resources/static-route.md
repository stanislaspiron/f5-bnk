```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKStaticRoute
metadata:
  name: default-route
  namespace: f5-bnk
spec:
  destination: 0.0.0.0
  prefixLen: 0
  type: gateway
  gateway: 10.245.2.1
EOF
```

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKStaticRoute
metadata:
  name: route-10
  namespace: f5-bnk
spec:
  destination: 10.0.0.0
  prefixLen: 8
  type: gateway
  gateway: 10.245.2.1
EOF
```