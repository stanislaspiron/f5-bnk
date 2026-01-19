# Variables

All this documentation uses following variables

## Create bnk-env.sh file

```bash
cat <<EOF > bnk-env.sh
f5_utils_namespace=f5-utils
f5_bnk_namespace=f5-bnk
f5_flp_namespace=f5-license-proxy
telemetry_namespace=f5-analytics
# f5_flo_version will be added by bnk manifest parsing. requires yq installed.
#f5_flo_version=v1.198.4-0.1.36
f5_bnk_version="2.1.0-3.1736.1-0.1.27"
env_proxy=http://1.2.3.4:8080
# For each node, 
# - nodes int_ip must match internal network IP address defined in /etc/netplan/Internal.yaml
# - nodes int_ip and tmms int_ip must be in same network.
# - tmms ext_ip must match external network IP address configured on connected routers

declare -A nodes=(\
 [1:name]=node1 [1:main_ip]=10.245.0.251 [1:int_ip]=172.16.245.251 [1:label]=workload1 \
 [2:name]=node2 [2:main_ip]=10.245.0.252 [2:int_ip]=172.16.245.252 [2:label]=workload2 \
 [3:name]=node3 [3:main_ip]=10.245.0.253 [3:int_ip]=172.16.245.253 [3:label]=f5-tmm\
)
# If dedicated is true, nodes with label f5-tmm will be tainted to dpu=true:NoSchedule
dedicated_tmm_nodes=false

declare -A tmm=(\
 [1:int_ip]=172.16.245.1 [1:ext_ip]=10.245.2.1 \
 [2:int_ip]=172.16.245.2 [2:ext_ip]=10.245.2.2 \
 [3:int_ip]=172.16.245.3 [3:ext_ip]=10.245.2.3 \
)
# Do not edit these lines
nodes[length]=0; for i in {1..10}; do if [ -n "${nodes[${i}:main_ip]}" ]; then ((nodes[length]++)); fi; done
tmm[length]=0; for i in {1..10}; do if [ -n "${tmm[${i}:int_ip]}" ]; then ((tmm[length]++)); fi; done



# Each interface name must match interface name listed in "ip -br a" command output
external_interface=External
#external_interface=ens192

internal_interface=Internal
#internal_interface=ens224

# pod_network must match pod-network-cidr option in kubeadm init
pod_network=192.168.0.0/16
# service_network must match service-cidr option in kubeadm init (default 10.96.0.0/12)
service_network=10.96.0.0/12

# NFS Configuration
nfs_server=10.171.61.18
nfs_path="/srv/nfs4/"

# License modes : 
# - connected
# - disconnected
# - f5licenseproxy
f5_license_mode=connected
# Copy here the license JWT
f5_licenseJWT=XXXXX
EOF
```

## load variables



### make this file loaded during login


```bash
echo "source bnk-env.sh" >> ~/.bashrc
```

### Reload bashrc

```bash
exec bash
```

