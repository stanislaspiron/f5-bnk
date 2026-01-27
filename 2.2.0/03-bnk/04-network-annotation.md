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
for (( i=1; i<=${nodes[length]}; i++ )); do
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
kubectl patch felixconfiguration default --type='merge' -p='{"spec": {"externalNodesList": ["'${tmm[1:int_ip]}'","'${tmm[2:int_ip]}'","'${tmm[3:int_ip]}'"]}}'
```

Verify Felix Configuration

```bash
kubectl get felixconfiguration default -o json | jq -r '.spec.externalNodesList'
```
[Back](03-networkAttachmentDefinition.md)  
[Next](05-cert-manager.md)