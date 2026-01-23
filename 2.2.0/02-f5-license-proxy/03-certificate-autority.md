
# Clusterissuer resources (Certificate autority)

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
    name: flp-root-ca
spec:
    selfSigned: {}
EOF
```
