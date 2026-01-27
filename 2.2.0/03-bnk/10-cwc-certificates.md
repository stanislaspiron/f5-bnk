# Cluster Wide Controller Certificates

The Cluster Wide Controller (CWC) enables software licensing and billing capabilities for BIG-IP Next for Kubernetes

## Requirement

**make** must be installed to run installation script.

```bash
sudo apt install make -y
```

## Login to helm registry
```bash
cat cne_pull_64.json | helm registry login -u _json_key_base64 --password-stdin repo.f5.com
```

## Download F5 Certificate manager from Helm registry

```bash
helm pull oci://repo.f5.com/utils/f5-cert-gen --version ${f5_cert_gen_version}
tar zxvf f5-cert-gen-${f5_cert_gen_version}.tgz
```

## Install F5 Cluster Wide Controller Certificates

```bash
sh cert-gen/gen_cert.sh -s=api-server -a=f5-spk-cwc.${f5_utils_namespace} -n=1
kubectl apply -f cwc-license-certs.yaml -n ${f5_utils_namespace}
```

# Alternate solution


```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: cwc-license-generate-cert
    namespace: ${f5_utils_namespace}
spec:
    subject:
        countries:
        - US
        provinces:
        - Washington
        localities:
        - Seattle
        organizations:
        - F5 Networks
        organizationalUnits:
        - Professional Services
    emailAddresses:
        - clientcert@f5net.com
    commonName: f5net.com
    dnsNames:
        - f5-spk-cwc.${f5_utils_namespace}
    secretName: cwc-license-certs-out
    issuerRef:
        name: bnk-intermediate-ca
        kind: ClusterIssuer
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 4096
    revisionHistoryLimit: 10    
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
    name: cwc-license-client-generate-cert
    namespace: ${f5_utils_namespace}
spec:
    subject:
        countries:
        - US
        provinces:
        - Washington
        localities:
        - Seattle
        organizations:
        - F5 Networks
        organizationalUnits:
        - Professional Services
    emailAddresses:
        - clientcert@f5net.com
    commonName: f5net.com
    dnsNames: 
        - f5-spk-cwc.${f5_utils_namespace}
        - f5-spk-cwc.${f5_utils_namespace}.svc.cluster.local
    secretName: cwc-license-client-certs-out
    issuerRef:
        name: bnk-intermediate-ca
        kind: ClusterIssuer
    duration: 8640h
    privateKey:
        rotationPolicy: Always
        encoding: PKCS1
        algorithm: RSA
        size: 4096
    revisionHistoryLimit: 10
EOF
```

Install F5 Cluster Wide Controller Certificates
```bash
kubectl get secret cwc-license-certs-out -n ${f5_utils_namespace} -o json | jq '{apiVersion: "v1",kind: "Secret",metadata: {name: "cwc-license-certs",namespace: "'${f5_utils_namespace}'"},type: "Opaque",data: {"ca-root-cert": .data["ca.crt"],"server-cert": .data["tls.crt"],"server-key": .data["tls.key"]}}' | kubectl apply -f -
kubectl get secret cwc-license-client-certs-out -n ${f5_utils_namespace} -o json | jq '{apiVersion: "v1",kind: "Secret",metadata: {name: "cwc-license-client-certs",namespace: "'${f5_utils_namespace}'"},type: "Opaque",data: {"ca-root-cert": .data["ca.crt"],"client-cert": .data["tls.crt"],"client-key": .data["tls.key"]}}' | kubectl apply -f -
```


[Back](09-otel-certificates.md)  
[Next](11-flo.md)