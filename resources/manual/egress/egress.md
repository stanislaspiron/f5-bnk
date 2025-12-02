# Egress

## Create Egress

### Variables
```bash
app_namespace=myapp
snat_prefix="10.245.3.11"
```

```bash
app_namespace=myapp2
snat_prefix="10.245.3.12"
```



## Snat Pools

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKSnatpool
metadata:
  name: egress-snatpool-${app_namespace}
  namespace: f5-bnk
spec:
  name: "egress-snatpool-${app_namespace}"
  sharedSnatAddressEnabled: false
  addressList:
    -  
      - ${snat_prefix}1
    - 
      - ${snat_prefix}2
    -
      - ${snat_prefix}3
EOF
```
### Egress configuration

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: k8s.f5net.com/v3
kind: F5SPKEgress
metadata:
  name: egress-${app_namespace}
  namespace: f5-bnk
spec:
  snatType: SRC_TRANS_SNATPOOL
  egressSnatpool: "egress-snatpool-${app_namespace}"
  pseudoCNIConfig:
    namespaces:
      - ${app_namespace}
    appPodInterface: eth0
    vxlan:
      create: true
      tmmInterfaceName: internal
#      key: 111
EOF
```

# Show egress

```bash
kubectl get f5-spk-egress -n f5-bnk egress-${app_namespace} -o yaml
```


# Delete egress

```bash
kubectl delete f5-spk-egress -n f5-bnk egress-${app_namespace}
```
