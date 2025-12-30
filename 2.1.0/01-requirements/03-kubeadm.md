
# Install kube requirements
```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
echo "Installing K8S dependencies on ${nodes[${i}:name]}"
ssh -q root@${nodes[${i}:main_ip]} 'bash -s'<<EOF
# Enable IP Forward
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# Disable SWAP
sudo swapoff -a
sudo sed -i '/swap/d' /etc/fstab

# Install required packages
sudo apt update && sudo apt upgrade -y
sudo apt install -y jq curl gnupg2 software-properties-common apt-transport-https ca-certificates

# Install Containerd package
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install -y containerd.io

# Configure Containerd
mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

# Restart containerd
sudo systemctl restart containerd && sudo systemctl enable containerd

# Install kubernetes packages
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | gpg --dearmor | sudo tee /usr/share/keyrings/kubernetes-archive-keyring.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list > /dev/null
sudo apt update && sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
EOF
done
```

# Install kubernetes master
```bash
ssh root@${nodes[1:main_ip]} 'bash -s'<<EOF
sudo kubeadm init --pod-network-cidr=${pod_network} --service-cidr=${service_network}
EOF
```

# Copy kubernetes configuration to user 

```bash
mkdir -p $HOME/.kube
echo "Installing K8S on ${nodes[1:name]}"
scp root@${nodes[1:main_ip]}:/etc/kubernetes/admin.conf $HOME/.kube/config
```

# Install kubernetes on worker nodes

```bash
join_command=$(ssh -q root@${nodes[1:main_ip]} kubeadm token create --print-join-command)
for (( i=2; i<=${nodes[length]}; i++ )); do
echo "Installing K8S on ${nodes[${i}:name]}"
ssh -q root@${nodes[${i}:main_ip]} ${join_command}
done
```


# Install Calico
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

# Check Nodes and Pods status
```bash
kubectl get nodes
kubectl get pods -A
```


# Install Multus
```bash
kubectl apply -f https://raw.githubusercontent.com/k8snetworkplumbingwg/multus-cni/master/deployments/multus-daemonset.yml
 ```

# Install Helm

```bash
sudo snap install helm --classic
```

# show nodes taint
```bash
kubectl get nodes -o=json | jq -r '.items[] | .metadata.name + "\t" + (if .spec.taints then (.spec.taints | map(.key + "=" + (.value // "") + ":" + .effect) | join(", ")) else "No taints" end)'
```

 # Remove control plan taint on master
```bash
kubectl taint nodes ${nodes[1:name]} node-role.kubernetes.io/control-plane-
```
