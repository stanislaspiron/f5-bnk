# Log publisher
Log publishers **MUST** be created in same namespace as BNKGatewayClass object.


```bash
cat <<EOF | kubectl apply -f -
apiVersion: k8s.f5net.com/v1
kind: F5BigLogHslpub
metadata:
  name: hsl-pub-${f5_bnk_namespace}
  namespace: ${f5_bnk_namespace}
spec:
  pool:
  - name: hsl-${f5_bnk_namespace}
    endpoint:
    - 10.245.2.74:514
  syslog:
  - name: syslog
    format: rfc5424
    protocol: tcp
    distribution: adaptive
    pool: hsl-${f5_bnk_namespace}
EOF
```
# Check Log publisher

```bash
kubectl get f5-big-log-hslpubs.k8s.f5net.com -n ${f5_bnk_namespace}
```

[Back](12-tmm-networking.md)


