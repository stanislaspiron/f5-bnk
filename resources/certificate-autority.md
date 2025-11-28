# Clusterissuer resources (Certificate autorities)

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
    name: app-root-ca
spec:
    selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: app-intermediate-ca
    namespace: cert-manager
spec:
    isCA: true
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
    commonName: app-intermediate-ca
    secretName: app-intermediate-ca
    issuerRef:
        name: app-root-ca
        kind: ClusterIssuer
        group: cert-manager.io
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: app-intermediate-ca
spec:
  ca:
    secretName: app-intermediate-ca
EOF
```