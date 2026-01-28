# Open Telemetry Certificates

Create OTEL certificates in the same namespace as BNK

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: external-otelsvr
    namespace: ${f5_utils_namespace}
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
    commonName: otel-collector-svc.${f5_utils_namespace}.svc.cluster.local
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - otel-collector-svc.${f5_utils_namespace}
    - otel-collector-svc.${f5_utils_namespace}.svc.cluster.local
    secretName: external-otelsvr-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: bnk-intermediate-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 1 hour, not configurable
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 4096
    revisionHistoryLimit: 10    
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: tls-otel-grpc-svr
    namespace: ${f5_utils_namespace}
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
    commonName: f5ingotelsvr.f5net.com
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    secretName: external-f5ingotelsvr-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: bnk-intermediate-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 360 days. 
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 4096
    revisionHistoryLimit: 10
EOF
```


[Back](08-Manifest.md)  
[Next](10-cwc-certificates.md)