# F5 BNK Manifest

Login to F5 Helm repo

```bash
cat cne_pull_64.json | helm registry login -u _json_key_base64 --password-stdin repo.f5.com
```

```bash
helm pull oci://repo.f5.com/release/f5-bigip-k8s-manifest --version ${f5_bnk_version}
tar xvzf f5-bigip-k8s-manifest-${f5_bnk_version}.tgz
```

```bash
manifest_file=f5-bigip-k8s-manifest-${f5_bnk_version}/bigip-k8s-manifest-${f5_bnk_version}.yaml
cat <<EOF >> bnk-env.sh
flp_version=$(yq -r '.releases.0.docker_images[] | select(.name == "images/f5-license-proxy"  ) | .version'  ${manifest_file})
EOF
```

Check pods status

```bash
kubectl get pod -n ${f5_flp_namespace} --watch
```