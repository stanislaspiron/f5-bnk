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
cat <<EOF >> bnk-env.sh
f5_flo_version=$(yq -r '.releases.0.docker_images[] | select(.name == "images/f5-lifecycle-operator"  ) | .version '  ${manifest_file})
f5_cert_gen_version=$(yq -r '.releases.0.helm_charts[] | select(.name == "utils/f5-cert-gen"  ) | .version '  ${manifest_file})
EOF
exec bash
```

[Back](07-far.md)  
[Next](09-otel-certificates.md)