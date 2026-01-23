# calico-node env variables : Get current values

```bash
kubectl get daemonset -n kube-system calico-node -o json | jq '.spec.template.spec.containers[0].env[] | select(.name == "IP_AUTODETECTION_METHOD" or .name == "CALICO_IPV4POOL_VXLAN")'
```

# calico-node env variables : Change values

```bash
kubectl set env  daemonsets.apps/calico-node  -n kube-system -c calico-node CALICO_IPV4POOL_VXLAN=CrossSubnet IP_AUTODETECTION_METHOD="skip-interface=vxlan*"
```

# ippool vxlanMode : Get current values

```bash
kubectl get  ippools.crd.projectcalico.org default-ipv4-ippool -o json | jq -r '.spec.vxlanMode'
```

# ippool vxlanMode : Change value

```bash
kubectl patch ippools.crd.projectcalico.org default-ipv4-ippool --type='merge'    -p '{"spec":{"vxlanMode":"CrossSubnet"}}'
```