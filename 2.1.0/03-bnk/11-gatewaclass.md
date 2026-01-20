# Gateway Class

GatewayClass is a cluster-scoped resource defined by the infrastructure provider. This resource represents a class of Gateways that can be instantiated. You must apply GatewayClass only once in a cluster. GatewayClass is applied at the global level.

```bash
cat << EOF | kubectl apply -f -
apiVersion: gateway.networking.k8s.io/v1
kind: GatewayClass
metadata:
  name: f5-gateway-class
spec:
  # include namespace in name
  controllerName: "f5.com/${f5_bnk_namespace}-f5-cne-controller"
  description: "F5 BIG-IP Kubernetes Gateway"
---
EOF
```

[Back](10-bnk.md)  
[Next](12-tmm-networking.md)