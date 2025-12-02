# Variables

All this documentation uses following variables

## Create bnk-env.sh file

```bash
cat <<EOF > bnk-env.sh
f5_utils_namespace=f5-utils
f5_bnk_namespace=f5-bnk
telemetry_namespace=f5-analytics
f5_flo_version=v1.198.4-0.1.36
f5_bnk_version="2.1.0-3.1736.1-0.1.27"
env_proxy=http://1.2.3.4:8080
# For each node, 
# - nodes int_ip must match internal network IP address defined in /etc/netplan/Internal.yaml
# - nodes int_ip and tmms int_ip must be in same network.
# - tmms ext_ip must match external network IP address configured on connected routers

declare -A nodes=(\
 [0:name]=node1 [0:main_ip]=10.245.0.251 [0:int_ip]=172.16.245.251 [0:label]=workload1 \
 [1:name]=node2 [1:main_ip]=10.245.0.252 [1:int_ip]=172.16.245.252 [1:label]=workload2 \
 [2:name]=node3 [2:main_ip]=10.245.0.253 [2:int_ip]=172.16.245.253 [2:label]=f5-tmm\
)

declare -A tmm=(\
 [0:int_ip]=172.16.245.81 [0:ext_ip]=10.245.2.1 \
 [1:int_ip]=172.16.245.82 [1:ext_ip]=10.245.2.2 \
 [2:int_ip]=172.16.245.83 [2:ext_ip]=10.245.2.3 \
)

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

