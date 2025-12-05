

## Generate SSH key
```bash
ssh-keygen
```

## Copy SSH key to nodes
```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
ssh-keygen -R ${nodes[${i}:main_ip]}
ssh-copy-id -o StrictHostKeyChecking=accept-new root@${nodes[${i}:main_ip]};
done
```

## Rename Nodes
```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
ssh root@${nodes[${i}:main_ip]} hostnamectl hostname ${nodes[${i}:name]} ;
done
export PS1='[\u@\h \W]\$'
exec bash
```

## Configure nodes network
```bash
for (( i=1; i<=${nodes[length]}; i++ )); do
ssh root@${nodes[${i}:main_ip]} tee /etc/netplan/90-external.yaml > /dev/null <<EOF
network:
  version: 2
  ethernets:
    ${external_interface}:
      addresses: []
      link-local: []
      mtu: 1500
EOF

ssh root@${nodes[${i}:main_ip]} tee /etc/netplan/90-internal.yaml > /dev/null <<EOF
network:
  version: 2
  ethernets:
    ${internal_interface}:
      dhcp4: no
      dhcp6: no
      link-local: []
    macvlan-host:
      addresses:
      - ${nodes[${i}:int_ip]}/24
      dhcp6: no
      link-local: []
EOF

ssh root@${nodes[${i}:main_ip]} tee /lib/systemd/system/macvlan.service  > /dev/null <<EOF
[Unit]
Description=Create macvlan interface before netplan
After=network.target

[Service]
ExecStart=-/sbin/ip link add macvlan-host address 02:f5:f5:f5:f5:f${i} link ${internal_interface} type macvlan mode bridge

[Install]
WantedBy=default.target
EOF

ssh root@${nodes[${i}:main_ip]} 'bash -s'<<EOF
chmod 600 /etc/netplan/90-*
netplan apply
systemctl daemon-reload
systemctl daemon-reexec
systemctl enable macvlan.service
systemctl restart macvlan.service
EOF
done
```