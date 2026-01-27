
# Create TMM VLANs

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKVlan
metadata:
  name: "external"
  namespace: ${f5_bnk_namespace}
spec:
  name: external
  mtu: 8000
  #tag: 203
  interfaces:
    - "1.1"
  selfip_v4s:
    - ${tmm[1:ext_ip]}
    - ${tmm[2:ext_ip]}
    - ${tmm[3:ext_ip]}
  prefixlen_v4: 24
  selfip_v6s: []
  prefixlen_v6: 96

---
apiVersion: "k8s.f5net.com/v1"
kind: F5SPKVlan
metadata:
  name: "internal"
  namespace: ${f5_bnk_namespace}
spec:
  name: internal
  mtu: 1350
  internal: true
  interfaces:
    - "1.2"
  selfip_v4s:
    - ${tmm[1:int_ip]}
    - ${tmm[2:int_ip]}
    - ${tmm[3:int_ip]}
  prefixlen_v4: 24
  selfip_v6s: []
  prefixlen_v6: 96
EOF
```

# Check Networking on TMM

```bash
kubectl exec -it daemonset/f5-tmm -c debug -n ${f5_bnk_namespace} -- bash
```


[Back](13-gatewaclass.md)  
[Next](15-log-publisher.md)