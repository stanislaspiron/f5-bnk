
# Clusterissuer resources (Certificate autorities)

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
    name: bnk-root-ca
spec:
    selfSigned: {}
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: bnk-intermediate-ca
    namespace: cert-manager
spec:
    isCA: true
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
    commonName: bnk-intermediate-ca
    secretName: bnk-intermediate-ca
    issuerRef:
        name: bnk-root-ca
        kind: ClusterIssuer
        group: cert-manager.io
    privateKey:
      rotationPolicy: Always
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: bnk-intermediate-ca
spec:
  ca:
    secretName: bnk-intermediate-ca
EOF
```


[Back](05-cert-manager.md)  
[Next](07-far.md)
