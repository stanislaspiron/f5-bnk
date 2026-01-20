# F5 License Proxy Root CA
This root CA must be added to FLO license.licenseserverrootca attribute


```bash
kubectl get secret flp-mtls-secret -n ${f5_flp_namespace} -o jsonpath='{.data.ca\.crt}' | base64 -d
```
