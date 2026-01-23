# Namespaces
During  BNK Deployment, F5 resources are created on two differents namespaces:
- Control plane resources : f5-utils
- Data plane resources : f5-bnk

```bash
kubectl create ns ${f5_utils_namespace}
kubectl create ns ${f5_bnk_namespace}
```


[Back](01-label.md)  
[Next](03-networkAttachmentDefinition.md)
