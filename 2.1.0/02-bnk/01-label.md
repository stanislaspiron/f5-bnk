# Add label f5-tmm on nodes

During the installation of BIG-IP Next for Kubernetes, the data plane components will be installed on cluster nodes that carry the ‘f5-tmm’ label. 

```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
  kubectl label node ${nodes[${i}:name]} app=${nodes[${i}:label]}
done
```

# Show node labels

```bash
kubectl get node --show-labels
```

# Optional : Taint DPU Nodes

Add a taint to the nodes to restrict these nodes to running only the TMM and prevent Control Plane workloads to be scheduled on the DPU nodes. This taint will still permit other system-level daemonsets, like Flannel, to be executed.

```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
  if [ $dedicated_tmm_nodes = true ]  && [ ${nodes[${i}:label]} = "f5-tmm" ]; 
    then 
      kubectl taint nodes ${nodes[${i}:name]} dpu=true:NoSchedule
    else 
      # disable dpu taint, ignore error if tain did not set previously
      kubectl taint nodes ${nodes[${i}:name]} dpu- 2> /dev/null
  fi;
done
```

result

```
node/node3 tainted
```

[Back](README.md)  
[Next](02-namespaces.md)