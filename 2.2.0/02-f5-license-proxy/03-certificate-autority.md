
# Clusterissuer resources (Certificate autority)

## Create Certificate autority from openssl
```bash
openssl req \
   -new \
   -newkey ec:<(openssl ecparam -name prime256v1) \
   -days 3650 -nodes -x509 \
   -subj "/C=US/ST=Washington/L=Seattle/O=F5/emailAddress=flp@f5.com/CN=FLP Root CA" \
   -keyout root-ca.key \
   -out root-ca.crt
```
## Import Certificate autority certificate and key

```bash
kubectl create secret generic -n cert-manager flp-root-ca-secret \
   --from-file=tls.crt=$HOME/root-ca.crt \
   --from-file=tls.key=$HOME/root-ca.key
```

## Create Certificate Autority from Certificate and key

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
    name: flp-root-ca
spec:
  ca:
    secretName: flp-root-ca-secret
EOF
```
