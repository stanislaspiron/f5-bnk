# Annotate Internal-facing interfaces

You must add an annotation to the Host node to establish static routes that direct traffic to the TMM pod on the DPU. The IP’s CIDR range must match the internal network’s CIDR range.

In this lab, TMM networks are configured with following componants:

|            | Internal     | External    |
|------------|--------------|-------------|
| node 1     | 10.245.1.251 |             |
| node 2     | 10.245.1.252 |             |
| node 3     | 10.245.1.253 |             |
| tmm pod 1  | 10.245.1.81  | 10.245.2.81 |
| tmm pod 2  | 10.245.1.82  | 10.245.2.82 |
| tmm pod 3  | 10.245.1.83  | 10.245.2.83 |


Use the following command to annotate the node (execute this for every nodes in the cluster):
```bash
for i in 0 1 2; do
kubectl annotate node ${nodes[${i}:name]} k8s.ovn.org/node-primary-ifaddr='{"ipv4":"'${nodes[${i}:int_ip]}'"}'
done
```
*This IP address must match Internal node IP*

Verify the node annotation.
```bash
kubectl get nodes -o yaml | grep node-primary-ifaddr
```

Update Felix Configuration to allow VXLAN ingress in Calico from Internal TMM self IPs

```bash
kubectl patch felixconfiguration default --type='merge' -p='{"spec": {"externalNodesList": ["'${tmm[0:int_ip]}'","'${tmm[1:int_ip]}'","'${tmm[2:int_ip]}'"]}}'
```
*This IP address must match Internal tmm self-IP*

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
    - ${tmm[0:ext_ip]}
    - ${tmm[1:ext_ip]}
    - ${tmm[2:ext_ip]}
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
    - ${tmm[0:int_ip]}
    - ${tmm[1:int_ip]}
    - ${tmm[2:int_ip]}
  prefixlen_v4: 24
  selfip_v6s: []
  prefixlen_v6: 96
EOF
```

# Check Networking on TMM

```bash
kubectl exec -it daemonset/f5-tmm -c debug -n ${f5_bnk_namespace} -- bash
```


[Back](11-gatewaclass.md)  
[Next](13-vxlan-disable-checksum.md)