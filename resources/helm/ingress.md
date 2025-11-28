# Install application from Helm Chart


## Variables
```
vip_name="vip10"
app_namespace="myapp"
app_vip="10.245.3.10"
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
    # -- Listener for HTTP
    - name: http
      protocol: HTTP
      port: 80
      allowedRoutes:
        namespaces:
          from: All
        kinds:
        - kind: HTTPRoute
    - name: tcp-8080
      protocol: TCP
      port: 8080
      allowedRoutes:
        namespaces:
          from: All
        kinds:
        - kind: L4Route
          group: gateway.k8s.f5net.com
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
  - name: irule-http-respond
    sectionName: http
    code: >
      when HTTP_REQUEST {
          HTTP::respond 200 content {This is a response from BNK iRule\n}
      }
EOF
```

## Install application

```bash
helm install gateway-${vip_name} helm-bnk-gateway/bnk-gateway --namespace myapp --values gateway-values-${vip_name}.yaml
```

## Unininstall application

```bash
helm uninstall gateway-${vip_name}  --namespace myapp
```