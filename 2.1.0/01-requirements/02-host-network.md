

## Generate SSH key
```bash
ssh-keygen
```

## Copy SSH key to nodes
```bash
for i in 0 1 2; do
ssh-keygen -R ${nodes[${i}:main_ip]}
ssh-copy-id -o StrictHostKeyChecking=accept-new root@${nodes[${i}:main_ip]};
done
```

## Rename Nodes
```bash
for i in 0 1 2; do
ssh root@${nodes[${i}:main_ip]} hostnamectl hostname ${nodes[${i}:name]} ;
done
export PS1='[\u@\h \W]\$'
exec bash
```

## Configure nodes network
```bash
for i in 0 1 2; do
ssh root@${nodes[${i}:main_ip]} tee  /etc/netplan/90-external.yaml > /dev/null <<EOF
network:
  version: 2
  ethernets:
    ${external_interface}:
      addresses: []
      link-local: []
      mtu: 1500
EOF

ssh root@${nodes[${i}:main_ip]} tee  /etc/netplan/90-internal.yaml > /dev/null <<EOF
network:
  version: 2
  ethernets:
    ${internal_interface}:
      addresses:
        - ${nodes[${i}:int_ip]}/24
      link-local: []
      mtu: 1500
EOF
ssh root@${nodes[${i}:main_ip]} 'bash -s'<<EOF
chmod 600 /etc/netplan/90-*
netplan apply
EOF
done
```