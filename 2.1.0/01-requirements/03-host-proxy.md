# System Proxy Configuration

## Configure proxy to environment variables
```bash
cat <<EOF > /etc/environment
HTTP_PROXY=${env_proxy}
HTTPS_PROXY=${env_proxy}
NO_PROXY=127.0.0.1,localhost,10.0.0.0/8,${service_network},${pod_network},svc.cluster.local,svc
EOF
```
## Configure proxy for package management (APT)
```bash
cat <<EOF > /etc/apt/apt.conf.d/95proxies
Acquire::http::Proxy ${env_proxy};
Acquire::https::Proxy ${env_proxy};
EOF
```

## Containerd proxy configuration

```bash
mkdir /etc/systemd/system/containerd.service.d
```

```bash
cat <<EOF >  /etc/systemd/system/containerd.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=${env_proxy}"
Environment="HTTPS_PROXY=${env_proxy}"
Environment="NO_PROXY=127.0.0.1,localhost,10.0.0.0/8,${service_network},${pod_network},svc.cluster.local,svc"
```
## Restart containerd to apply proxy configuration
```bash
systemctl daemon-reexec
systemctl daemon-reload
systemctl restart containerd.service
```
