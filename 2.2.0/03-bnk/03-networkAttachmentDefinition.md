## Network Attachment Definitions
A Network Attachment Definition is a Multus configuration that connects an underlying network (SF) to a Kubernetes pod (TMM).

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: k8s.cni.cncf.io/v1
kind: NetworkAttachmentDefinition
metadata:
  name: external-net
  namespace: ${f5_bnk_namespace}
spec:
  config: |
    {
      "cniVersion": "0.3.1",
      "type": "macvlan",
      "master": "${external_interface}",
      "mode": "bridge",
      "ipam": {},
      "logLevel": "debug",
      "logFile": "/var/log/net-attach-external.log"
    }
---
apiVersion: k8s.cni.cncf.io/v1
kind: NetworkAttachmentDefinition
metadata:
  name: internal-net
  namespace: ${f5_bnk_namespace}
spec:
  config: |
    {
      "cniVersion": "0.3.1",
      "type": "macvlan",
      "master": "${internal_interface}",
      "mode": "bridge",
      "ipam": {},
      "logLevel": "debug",
      "logFile": "/var/log/net-attach-internal.log"
    }
EOF
```


[Back](02-namespaces.md)  
[Next](04-network-annotation.md)
