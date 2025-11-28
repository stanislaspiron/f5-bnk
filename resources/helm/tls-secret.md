# Create VIP with HTTP and HTTPS virtual server

Requires [certificate autority created first](../certificate-autority.md)

## Variables
```
vip_name="vip10"
app_namespace="myapp"
app_vip="10.245.3.10"
```

# Create TLS secret

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: certificate-${vip_name}
    namespace: ${app_namespace}
spec:
    subject:
        countries:
        - FR
        provinces:
        - Ile de France
        localities:
        - Suresnes
        organizations:
        - F5 Networks
        organizationalUnits:
        - Professional Services
    emailAddresses:
        - ps@f5.com
    commonName: "${vip_name}.demo.net"
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - "${vip_name}.demo.net"
    - "${vip_name}"
    ipAddresses:
    - ${app_vip}
    secretName:  tls-secret-${vip_name}
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
        size: 2048
    revisionHistoryLimit: 10
EOF
```

# List resources

```bash
kubectl get tls -n ${app_namespace}
```
