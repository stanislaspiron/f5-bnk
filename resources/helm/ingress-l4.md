# Install application from Helm Chart


## Variables
```
vip_name="vip11"
app_namespace="myapp"
app_vip="10.245.3.11"
```


## Create Helm values

```bash
cat <<EOF > gateway-values-${vip_name}.yaml
# -- Gateway API Configuration
gateway:
  # -- Enable Gateway API
  enabled: true
  # -- Gateway name. Default: helm release name
  name: ${vip_name}
  # -- Gateway Class name. Reference to the GatewayClass object. Default: helm release name.
  gatewayClassName: f5-gateway-class
  addresses:
  - ${app_vip}
  # -- Define listeners. Use any format according to the original Gateway API spec.
  listeners:
    - name: tcp-8080
      protocol: TCP
      port: 8080
      allowedRoutes:
        namespaces:
          from: All
        kinds:
        - kind: L4Route
          group: gateway.k8s.f5net.com
l4Route:
  enabled: true
  items:
  - name: l4-tcp-${vip_name}
    protocol: TCP
    parentRefs:
    - name: ${vip_name}
      sectionName: tcp-8080
    rules:
    - backendRefs:
      - name: nginx-service
        port: 80
EOF
```

## Install application

```bash
helm install gateway-${vip_name} helm-bnk-gateway/bnk-gateway --namespace ${app_namespace} --values gateway-values-${vip_name}.yaml
```

## Unininstall application

```bash
helm uninstall gateway-${vip_name}  --namespace ${app_namespace}
```