# Postgres Certificate

Create certificate in the same namespace as FLP


```bash
kubectl create secret -n ${f5_flp_namespace} generic flp-jwt-secret --from-literal=jwt_secret=${f5_licenseJWT} --dry-run=client -o yaml | kubectl apply -f -
```