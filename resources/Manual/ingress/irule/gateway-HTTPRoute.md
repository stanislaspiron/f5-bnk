# Create VIP with HTTP virtual server

## Variables
```
vip_name="vip-irule"
app_namespace="myapp"
app_vip="10.245.3.3"
app_irule_name="irule-http-respond"
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
  - name: http
    protocol: HTTP
    port: 80
    allowedRoutes:
      namespaces:
        from: "All"
      kinds:
      - kind: HTTPRoute
EOF
```

## Create HTTP Route
```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: l7-http-${vip_name}
  namespace:  ${app_namespace}
spec:
  parentRefs:
  - name: ${vip_name}
    sectionName: http
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
kubectl get gateways.gateway.networking.k8s.io -n  ${app_namespace}
```

## HTTPRoute
```bash
kubectl get httproutes.gateway.networking.k8s.io -n  ${app_namespace}
```