# Install application from Helm Chart


## Variables
```
vip_name="vip12"
app_namespace="myapp"
app_vip="10.245.3.12"
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
    - name: https
      port: 443
      protocol: HTTPS
      tls:
        mode: Terminate
        certificateRefs:
        - name: tls-secret-${vip_name}
          kind: Secret
httpRoute:
  enabled: true
  items:
  - name: l7-http-${vip_name}
    parentRefs:
    - name: ${vip_name}
      sectionName: https
    - name: ${vip_name}
      sectionName: http
    hostnames:
    - app10.demo.local
    rules:
    - backendRefs:
      - name: nginx-service
        port: 80
irule:
  enabled: true
  items:
  - name: irule-ping
    sectionName: https
    code: >
          when HTTP_REQUEST {
            if {[HTTP::path] == "/ping"}{
              HTTP::respond 200 content {pong\n}
            }
          }
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