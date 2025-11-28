# Egress

## Snat Pools

### namespace app1
```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKSnatpool
metadata:
  name: egress-snatpool-app1
  namespace: f5-bnk
spec:
  name: "egress-snatpool-app1"
  sharedSnatAddressEnabled: false
  addressList:
    -  
      - 10.245.3.121
    - 
      - 10.245.3.122
    -
      - 10.245.3.123
EOF
```
### namespace app2

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKSnatpool
metadata:
  name: egress-snatpool-app2
  namespace: f5-bnk
spec:
  name: "egress-snatpool-app2"
  sharedSnatAddressEnabled: false
  addressList:
    -  
      - 10.245.3.131
    - 
      - 10.245.3.132
    -
      - 10.245.3.133
EOF
```

## Create Egress

### Variables
```bash
app_namespace=app1
```

```bash
app_namespace=app2
```

### configuration

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
      key: 101
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
