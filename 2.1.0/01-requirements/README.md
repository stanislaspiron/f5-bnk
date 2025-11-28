# F5 BNK Lab guide

This documentation describes how to deploy F5 BNK on K3S cluster

Minimum system requirement for Demo mode are
- Single Node:
    16 vCPU / 48 GB RAM
- 2 Nodes:
    12 vCPU / 32 GB RAM
- 3 Nodes:
    8 vCPU / 32 GB RAM

This documentation does not define minimum node local storage, but most of kubernetes cluster for LAB I deployed are provisioned with at least 60GB disk.

Each Node must be provisioned with 3 network Interfaces:
- K8S management interface
- TMM Internal interface
- TMM External interface


Note: 
> Every network interface must be enabled in linux configuration.  
> Node Internal interface must be configured with IPv4 address for egress feature.  
> When deploying on VMWare, update [VMWare port group security settings](01-requirements/vmware-network-security.md)

## Lab deployment

1. Configure nodes network interfaces
    - [Dedicated f5-tmm nodes](01-requirements/host-network-ubuntu.md)
    - [App pods and f5-tmm running on same nodes](01-requirements/host-network-ubuntu-macvlan.md)
1. Rename all Hosts `echo newHostname | sudo tee /dev/hostname`. Reboot device
1. Install Kubernetes cluster
    - [K3S](01-requirements/k3s-install.md) - This installation does not support egress
    - [Kubeadm](01-requirements/kubeadm.md)
    - [Kubeadm with proxy](01-requirements/kubeadm-proxy.md)
1. [Configure NFS storageClass](01-requirements/nfs-storageClass.md)
1. Install BNK
    - [BNK version 2.0.0](./02-bnk/2.0.0/README.md)
    - [BNK version 2.1.0](./02-bnk/2.1.0/README.md)
    - [BNK version 2.1.0 (variables)](./02-bnk/2.1.0-tmpl/README.md)
1. Telemetry
    1. [Install prometheus](03-resources/telemetry/prometheus.md)
    2. [Install Grafana](03-resources/telemetry/grafana.md)
    3. [Install Grafana](03-resources/telemetry/dashboard.md)


## Create BNK resources

- [Install Helm repo for BNK resources](charts/README.md)

- Ingress
    - [NGINX Application](03-resources/ingress/nginx-app.md)
    - [HTTP / HTTPS VIP](03-resources/ingress/multiple-services/gateway-HTTPRoute.md)
    - [L4 VIP](03-resources/ingress/l4/gateway-l4-route.md)
    - [HTTP VIP with irule](03-resources/ingress/irule/gateway-HTTPRoute.md)
    - [HTTP with Helm Chart](03-resources/helm/ingress.md)
- Egress
    - [Egress with Helm Chart](03-resources/helm/egress.md)

- [TLS Secrets for ingress resources](03-resources/helm/tls-secret.md)

## Additional resources

- [Patch Calico configuration](01-requirements/patch-calico.md)
- [External NFS configuration](01-requirements/nfs-server.md)
- [VMWare port group security settings](01-requirements/vmware-network-security.md)
- [Increase memory for init-f5-toda-tmstats](01-requirements/patch-memory-init-f5-toda-tmstats.md)
- [License operations](./02-bnk/bnk-license.md)
- [Create Certificate autority for apps](03-resources/certificate-autority.md)
