# F5 License Proxy

## F5 License Proxy installation

Login to F5 Helm repo

```bash
cat cne_pull_64.json | helm registry login -u _json_key_base64 --password-stdin repo.f5.com
```

```bash
helm install f5-license-proxy oci://repo.f5.com/charts/f5-license-proxy \
--version ${flp_version} \
--namespace ${f5_flp_namespace} \
--timeout 600s \
--set flp.imagePullSecrets=far-secret \
--set flp.image.repository=repo.f5.com/images \
--set vaultInit.image.repository=repo.f5.com/images \
--set vault.image.repository=repo.f5.com/images \
--set postgresql.image.repository=repo.f5.com/images
```

Check pods status

```bash
kubectl get pod -n ${f5_flp_namespace} --watch
```