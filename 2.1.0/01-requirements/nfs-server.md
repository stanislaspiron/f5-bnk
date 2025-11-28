# External NFS server deployment

## Requirements
My NFS server is deployed on Ubuntu 22.04
- 2 vCPU
-  2GB RAM

## NFS Installation

```bash
sudo apt install nfs-kernel-server
```

## NFS directory
Create **/srv/nfs4** directory

```bash
sudo mkdir /srv/nfs4
```


## NFS configuration for anonymous access

Edit the **/etc/exports** file and add the following line
```
/srv/nfs4     *(rw,sync,no_root_squash,subtree_check)
```

## Apply configuration

```bash
sudo exportfs -a
```
