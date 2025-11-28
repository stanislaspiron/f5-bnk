# Create Virtual server

## Variables
```
vip_name="vip2"
app_namespace="myapp"
app_vip="10.245.3.2"
```

## Gateway (Virtual servers)

```bash
cat <<EOF | kubectl apply -f -
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: ${vip_name}
  namespace: ${app_namespace}
spec:
  addresses:
  - type: "IPAddress"
    value: ${app_vip}
  gatewayClassName: f5-gateway-class
  listeners:
  - name: tcp-80
    protocol: TCP
    port: 80
    allowedRoutes:
      kinds:
      - kind: L4Route
        group: gateway.k8s.f5net.com
EOF
```

## Create L4 Route
```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: gateway.k8s.f5net.com/v1
kind: L4Route
metadata:
  name: l4-tcp-${vip_name}
  namespace:  ${app_namespace}
spec:
  protocol: TCP
  parentRefs:
  - name: ${vip_name}
    sectionName: tcp-80
  rules:
  - backendRefs:
    - name: nginx-service
      namespace: myapp
      port: 80
EOF
```

# List resources
## Gateway
```bash
kubectl get gateways.gateway.networking.k8s.io -n ${app_namespace}
```

## L4Route
```bash
kubectl get L4Route.gateway.networking.k8s.io -n ${app_namespace}
```