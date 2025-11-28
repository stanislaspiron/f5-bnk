# F5 BNK Lab guide

This documentation describes how to deploy F5 BNK on Kubernetes Cluster

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

For requirements, including Kubernetes deployment, installation commands are for Ubuntu 22.04. All kubernetes commands to deploy BNK are working on any system.

Note: 
> Every network interface must be enabled in linux configuration.  
> Node Internal interface must be configured with IPv4 address for egress feature.  
> When deploying on VMWare, update [VMWare port group security settings](01-requirements/vmware-network-security.md)

## Lab deployment

1. Configure nodes network interfaces
    - [Dedicated f5-tmm nodes](2.1.0/01-requirements/02-host-network.md)
    - [App pods and f5-tmm running on same nodes](2.1.0/01-requirements/host-network-ubuntu-macvlan.md)
1. Install Kubernetes cluster
    - [Kubeadm](2.1.0/01-requirements/03-kubeadm.md)
1. [Configure NFS storageClass](2.1.0/01-requirements/04-nfs-storageClass.md)
1. Install BNK
    - [BNK version 2.1.0](./2.1.0/02-bnk/README.md)
1. Telemetry
    1. [Install prometheus](2.1.0/resources/telemetry/prometheus.md)
    2. [Install Grafana](2.1.0/resources/telemetry/grafana.md)
    3. [Install Grafana](2.1.0/resources/telemetry/dashboard.md)


## Create BNK resources

- [Install Helm repo for BNK resources](charts/README.md)

- Ingress
    - [NGINX Application](resources/manual/ingress/nginx-app.md)
    - [HTTP / HTTPS VIP](resources/manual/ingress/multiple-services/gateway-HTTPRoute.md)
    - [L4 VIP](resources/manual/ingress/l4/gateway-l4-route.md)
    - [HTTP VIP with irule](resources/manual/ingress/irule/gateway-HTTPRoute.md)
    - [HTTP with Helm Chart](resources/helm/ingress.md)
- Egress
    - [Egress with Helm Chart](resources/helm/egress.md)

- [TLS Secrets for ingress resources](resources/helm/tls-secret.md)

## Additional resources

- [Patch Calico configuration](2.1.0/01-requirements/05-patch-calico.md)
- [External NFS configuration](2.1.0/01-requirements/04-nfs-server.md)
- [VMWare port group security settings](2.1.0/01-requirements/vmware-network-security.md)
- [Increase memory for init-f5-toda-tmstats](2.1.0/01-requirements/patch-memory-init-f5-toda-tmstats.md)
- [License operations](./02-bnk/bnk-license.md)
- [Create Certificate autority for apps](resources/certificate-autority.md)
