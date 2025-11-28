# Install application from Helm Chart

## Variables
```
vip_name="vip10"
app_namespace="myapp"
snat_prefix="10.245.3.11"
```

## Create Helm values

```bash
cat <<EOF > egress-values.yaml
egress:
  enabled: true
  isolation: true
#  vxlanKey: 101
  addresses:
    -  
      - ${snat_prefix}1
    - 
      - ${snat_prefix}2
    -
      - ${snat_prefix}3
EOF
```

## Install application

```bash
helm install bnk-egress helm-bnk-gateway/bnk-gateway --namespace myapp --values egress-values.yaml
```

## Unininstall application

```bash
helm uninstall bnk-egress  --namespace myapp
```


# Show egress

```bash
kubectl get f5-spk-egress -n f5-bnk egress-myapp -o yaml
```

# Show snatpool

```bash
kubectl get F5-SPK-Snatpool -n f5-bnk  egress-snatpool-myapp  -o yaml
```



# Delete egress

```bash
kubectl delete f5-spk-egress -n f5-bnk egress-myapp
```
