# Global context policy

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5BigFwPolicy
metadata:
  name: gatewayclass-global-policy
  namespace: f5-bnk
spec:
  rule:
  - name: dst-rule-tcp-egress
    ipProtocol: any
    source:
      addresses:
      - 10.0.0.0/16
    destination:
      addresses:
      - 10.245.2.74/32
    logging: true
    action: drop

---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
kind: BNKSecPolicy
metadata:
  name: gatewayclass-global-security
  namespace: f5-bnk
spec:
  extensionRefs:
  - kind: F5BigFwPolicy
    name: gatewayclass-global-policy
  targetRefs:
  - kind: GatewayClass
    name: f5-gateway-class
    sectionName: ""
EOF
```

# Per-egress policy (2.1.0-EHF-2)

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5BigFwPolicy
metadata:
  name: gatewayclass-global-policy
  namespace: f5-bnk
spec:
  rule:
  - name: dst-rule-tcp-egress
    ipProtocol: any
    source:
      addresses:
      - 10.0.0.0/16
    destination:
      addresses:
      - 10.245.2.74/32
    logging: true
    action: drop

---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
kind: BNKSecPolicy
metadata:
  name: gatewayclass-global-security
  namespace: f5-bnk
spec:
  extensionRefs:
  - kind: F5BigFwPolicy
    name: gatewayclass-global-policy
  targetRefs:
  - kind: GatewayClass
    name: f5-gateway-class
    sectionName: ""
EOF
```


# List AFM Policies
```bash
kubectl get f5bigfwpolicy.k8s.f5net.com -A
```

# List AFM Policy association
```bash
kubectl get bnksecpolicy.gateway.k8s.f5net.com -A
```
