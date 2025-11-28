# Add label f5-tmm on nodes

During the installation of BIG-IP Next for Kubernetes, the data plane components will be installed on cluster nodes that carry the ‘f5-tmm’ label. 

```bash
for i in 0 1 2; do
kubectl label node ${nodes[${i}:name]} app=${nodes[${i}:label]}
done
```

# Show node labels

```bash
kubectl get node --show-labels
```

# Optional : Taint DPU Nodes

Add a taint to the nodes to restrict these nodes to running only the TMM and prevent Control Plane workloads to be scheduled on the DPU nodes. This taint will still permit other system-level daemonsets, like Flannel, to be executed.

```
kubectl taint nodes kube-node1 dpu=true:NoSchedule
```


[Back](README.md)  
[Next](02-namespaces.md)