# Proxy configuration

Create Proxy configuration

```bash
kubectl create secret generic flp-secret \
   --from-literal=host=<proxy_host> \
   --from-literal=port=<proxy_port> \
   --from-literal=protocol=http \
   --from-literal=username=<username> \
   --from-literal=password=<password>
```

Add Proxy TLS decryption CA certificate

```bash
kubectl create secret generic flp-ssl-secret \
   --from-file=ca.crt=<path_to_ca.crt>
```

Update FLP configuration

Login to F5 Helm repo

```bash
cat cne_pull_64.json | helm registry login -u _json_key_base64 --password-stdin repo.f5.com
```

```bash
helm update f5-license-proxy oci://repo.f5.com/charts/f5-license-proxy \
--version ${flp_version} \
--namespace ${f5_flp_namespace} \
--set flp.isProxyTlsEnabled=true \
--set flp.isProxyAuthEnabled=true
```