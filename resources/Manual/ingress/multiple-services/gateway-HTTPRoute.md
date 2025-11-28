# Create VIP with HTTP and HTTPS virtual server

## Variables
```
vip_name="vip1"
app_namespace="myapp"
app_vip="10.245.3.1"
app_irule_name="irule-ping"
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
  - name: https
    protocol: HTTPS
    port: 443
    tls:
      certificateRefs:
      - kind: Secret
        group: ""
        name: tls-secret
        namespace: ${app_namespace}
    allowedRoutes:
      namespaces:
        from: "All"
      kinds:
      - kind: HTTPRoute
EOF
```

# Create HTTP Route

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
  - name: ${vip_name}
    sectionName: https
  rules:
  - matches:
    - headers:
      - name: "Host"
        value: "blue.demo.net"
    backendRefs:
    - name: nginx-service
      namespace: ${app_namespace}
      port: 80
EOF
```

# Create TLS secret



## Certificate from cert manager
```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: app-certificate
    namespace: ${app_namespace}
spec:
    subject:
        countries:
        - US
        provinces:
        - Washington
        localities:
        - Seattle
        organizations:
        - F5 Networks
        organizationalUnits:
        - PD
    emailAddresses:
        - clientcert@f5net.com
    commonName: "*.demo.net"
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - "*.demo.net"
    secretName:  tls-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: app-intermediate-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 1 hour, not configurable
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 4096
    revisionHistoryLimit: 10
EOF
```

## Certificate with openssl

```bash
openssl req -nodes -x509 -newkey rsa:4096 -keyout ./server.key -out ./server.pem -sha256 -days 365 -subj '/CN=*.demo.net'
```

## import secret to namespace

```bash
kubectl create secret tls tls-secret --cert=server.pem --key=server.key -n ${app_namespace}
```

# List resources
## Gateway
```bash
kubectl get gateways.gateway.networking.k8s.io -n ${app_namespace}
```

## HTTPRoute
```bash
kubectl get httproutes.gateway.networking.k8s.io -n ${app_namespace}
```