# Create irule

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5BigCneIrule
metadata:
  name: ${app_irule_name}
  namespace: ${app_namespace}
spec:
  iRule: >
    when HTTP_REQUEST {
        HTTP::respond 200 content {This is a response from BNK iRule\n}
    }
EOF
```

# Assign irule to virtual server

```bash
cat <<EOF | kubectl apply -f -
apiVersion: gateway.k8s.f5net.com/v1alpha1
kind: BNKNetPolicy
metadata:
  name: netpolicy-${vip_name}
  namespace: ${app_namespace}
spec:
  extensionRefs:
  - kind: F5BigCneIrule
    name: ${app_irule_name}
  targetRefs:
  - kind: Gateway
    name: ${vip_name}
    sectionName: http
    group: gateway.networking.k8s.io
EOF
```
