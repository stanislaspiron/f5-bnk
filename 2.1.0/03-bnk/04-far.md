# F5 Artifact Registry

The BIG-IP Next for Kubernetes manifest file, Helm charts, Docker images, and other utilities are accessible through the F5 Artifact Registry (FAR) at repo.f5.com. A valid Service Account Key is required to access FAR.

This document details the procedures for downloading a Service Account Key, and using the Service Account Key to download the Manifest file and install Helm charts, docker images, and other utilities into the cluster from FAR or Private Registry.

## download from F5 Downloads




Download f5-far-auth-key.tar from F5 Downloads.  
Select Group  BIG-IP Next / BIG-IP Next for Kubernetes / Version 2.1.0 / f5-far-auth-key.tgz --> Click on Copy Link Download

```bash
curl  -o f5-far-auth-key.tgz '<download link url>'
```

Decompress file.

```bash
tar xzvf f5-far-auth-key.tgz
```

## FAR secret for images downloads

### FAR secret creation from far key
Create following script to create FAR secret from key script name : **far-secret.sh**

```bash
#!/bin/bash

# Read the content of pipeline.json into the SERVICE_ACCOUNT_KEY variable
SERVICE_ACCOUNT_KEY=$(cat cne_pull_64.json)

# Create the SERVICE_ACCOUNT_K8S_SECRET variable by appending "_json_key_base64:" to the base64 encoded SERVICE_ACCOUNT_KEY
SERVICE_ACCOUNT_K8S_SECRET=$(echo "_json_key_base64:${SERVICE_ACCOUNT_KEY}" | base64 -w 0)

# Create the secret.yaml file with the provided content
cat << EOF > far-secret.yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: far-secret
data:
  .dockerconfigjson: $(echo "{\"auths\": {\
\"repo.f5.com\":\
{\"auth\": \"$SERVICE_ACCOUNT_K8S_SECRET\"}}}" | base64 -w 0)
type: kubernetes.io/dockerconfigjson
EOF
```

### Execute the far-secret.sh to create the far-secret.yaml file

```bash
sh far-secret.sh
```

### Create far secret

The FAR secret is used by F5 deployements to authenticate on F5 artifactory. it must be created on **ALL** namespaces with F5 deployements.
```bash
kubectl create -f far-secret.yaml -n ${f5_utils_namespace}
kubectl create -f far-secret.yaml -n ${f5_bnk_namespace}
```


[Back](03-networkAttachmentDefinition.md)  
[Next](05-cert-manager.md)
