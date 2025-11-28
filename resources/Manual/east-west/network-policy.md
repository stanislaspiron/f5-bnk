# Create VIP with HTTP virtual server

## Variables
```
app_namespace="myapp"
```

## Gateway (Virtual servers)
```bash
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ns-isolation
  namespace: ${app_namespace}
spec:
  podSelector: {}  # Apply to all pods in namespace
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
      - podSelector: {}  # Allow all pods in the same namespace
    - from:
      - ipBlock:
          cidr: 10.245.1.81/32  # allow ingress from TMM internal self IP
  egress:
    - to:
      - ipBlock:
          cidr: 0.0.0.0/0  # allow egress to external IPs
EOF
```