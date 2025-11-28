# HTTP Policy

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "k8s.f5net.com/v1"
kind: F5BigFwPolicy
metadata:
  name: fw-policy-drop-all
  namespace: ${app_namespace}
spec:
  rule:
  - name: drop-all
    ipProtocol: any
    source:
      addresses: ["0.0.0.0/0","::/0"]
    logging: true
    action: drop
```

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
kind: BNKSecPolicy
metadata:
  name: secpolicy-${vip_name}-http
  namespace: ${app_namespace}
spec:
  extensionRefs:
  - name: fw-policy-drop-all
    group: k8s.f5net.com
    kind: F5BigFwPolicy
  targetRefs:
  - kind: Gateway
    name: ${vip_name}
    sectionName: "http"
EOF
```

# HTTPS Policy

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5BigFwPolicy
metadata:
  name: fw-policy-${vip_name}
  namespace: ${app_namespace}
spec:
  rule:
  - name: accept-from-jumphost
    ipProtocol: any
    source:
      addresses:
      - 10.245.2.74
    logging: true
    action: accept
  - name: drop-all
    ipProtocol: any
    source:
      addresses: ["0.0.0.0/0","::/0"]
    logging: true
    action: drop
EOF
```

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
kind: BNKSecPolicy
metadata:
  name: secpolicy-${vip_name}-https
  namespace: ${app_namespace}
spec:
  extensionRefs:
  - name: fw-policy-${vip_name}
    group: k8s.f5net.com
    kind: F5BigFwPolicy
  targetRefs:
  - kind: Gateway
    name: ${vip_name}
    sectionName: "https"
EOF
```

# List AFM Policies
```bash
kubectl get f5bigfwpolicy.k8s.f5net.com -n ${app_namespace}
```

# List AFM Policy association
```bash
kubectl get bnksecpolicy.gateway.k8s.f5net.com -n ${app_namespace}
```
