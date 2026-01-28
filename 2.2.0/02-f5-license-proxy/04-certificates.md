# Postgres Certificate

Create certificate in the same namespace as FLP


```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: postgresql
    namespace: ${f5_flp_namespace}
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
        - flp@f5net.com
    commonName: postgresql.${f5_flp_namespace}.svc.cluster.local
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - localhost
    - postgresql
    - postgresql.${f5_flp_namespace}
    - postgresql.${f5_flp_namespace}.svc.cluster.local
    secretName: postgresql-mtls-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: flp-root-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 1 hour, not configurable
    duration: 2160h # 90d
    renewBefore: 360h # 15d
    privateKey:
        rotationPolicy: Always
        algorithm: ECDSA
        size: 256
    revisionHistoryLimit: 10
EOF
```

# Vault Certificate

Create certificate in the same namespace as FLP


```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: vault
    namespace: ${f5_flp_namespace}
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
        - flp@f5net.com
    commonName: vault
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - localhost
    - vault
    - vault-postgresql-service
    - vault-postgresql-service.${f5_flp_namespace}
    - vault-postgresql-service.${f5_flp_namespace}.svc.cluster.local
    secretName: vault-ssl-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: flp-root-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 360 days. 
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 8192
    revisionHistoryLimit: 10
EOF
```

# FLP Certificate

Create certificate in the same namespace as FLP


```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: flp
    namespace: ${f5_flp_namespace}
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
        - flp@f5net.com
    commonName: flp
    # SecretName is the name of the secret resource that will be automatically created and managed by this Certificate resource.
    # It will be populated with a private key and certificate, signed by the denoted issuer.
    dnsNames:
    - localhost
    - flp
    - flp.${f5_flp_namespace}
    - flp.${f5_flp_namespace}.svc.cluster.local
    - flp.demo.local
    ipAddresses:
    - 10.171.61.45
    secretName: flp-mtls-secret
    # IssuerRef is a reference to the issuer for this certificate.
    issuerRef:
        name: flp-root-ca
        kind: ClusterIssuer
    # Lifetime of the Certificate is 360 days. 
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 8192
    revisionHistoryLimit: 10
EOF
```