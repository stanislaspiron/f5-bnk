# HTTPS Policy

```bash
cat <<EOF | kubectl apply -f -
apiVersion: "k8s.f5net.com/v1"
kind: F5BigFwPolicy
metadata:
  name: fw-policy-${vip_name}
  namespace: ${app_namespace}
spec:
  rule:
  - name: accept-from-jumphost
    ipProtocol: any
    source:
      addresses:
      - 10.245.2.74
    logging: true
    action: accept
  - name: drop-all
    ipProtocol: any
    source:
      addresses: ["0.0.0.0/0","::/0"]
    logging: true
    action: drop
EOF
```
# Log publisher

```bash
cat <<EOF | kubectl apply -f -
apiVersion: k8s.f5net.com/v1
kind: F5BigLogHslpub
metadata:
  name: hsl-pub-${app_namespace}
  namespace: ${app_namespace}
spec:
  pool:
  - name: hsl-${app_namespace}
    endpoint:
    - 10.245.2.74:514
  syslog:
  - name: syslog
    format: rfc5424
    protocol: tcp
    distribution: adaptive
    pool: hsl-${app_namespace}
EOF
```

# Log Profile

```bash
cat <<EOF | kubectl apply -f -
apiVersion: k8s.f5net.com/v2
kind: F5BigLogProfile
metadata:
  name: logprofile-${app_namespace}
  namespace: ${app_namespace}
spec:
  firewall:
    enabled: true
    network:
      publisher: hsl-pub-${app_namespace}
      events:
        aclMatchAccept: true
        aclMatchDrop: true
        aclMatchReject: true
        translationFields: true
      format:
        type: field-list
        networkFieldList:
          delimiter: ";"
          items:
            - "date_time"
            - "bigip_hostname"
            - "management_ip_address"
            - "acl_policy_name"
            - "src_ip"
            - "src_port"
            - "dest_ip"
            - "dest_port"
            - "translated_src_ip"
            - "translated_dest_ip"
            - "translated_src_port"
            - "translated_dest_port"
            - "protocol"
            - "action"
            - "context_name"
EOF
```

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "gateway.k8s.f5net.com/v1"
kind: BNKSecPolicy
metadata:
  name: secpolicy-${vip_name}
  namespace: ${app_namespace}
spec:
  extensionRefs:
  - name: fw-policy-${vip_name}
    group: k8s.f5net.com
    kind: F5BigFwPolicy
  - kind: F5BigLogProfile
    name: logprofile-${app_namespace}
    group: k8s.f5net.com
  targetRefs:
  - kind: Gateway
    name: ${vip_name}
    group: gateway.networking.k8s.io
EOF
```

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
kind: BNKSecPolicy
metadata:
  name: secpolicy-log-${vip_name}
  namespace: ${app_namespace}
spec:
  extensionRefs:
  - kind: F5BigLogProfile
    name: logprofile-${app_namespace}
    group: k8s.f5net.com
  targetRefs:
  - kind: Gateway
    name: ${vip_name}
EOF
```

# List AFM Policies
```bash
kubectl get f5bigfwpolicy.k8s.f5net.com -n ${app_namespace}
```

# List AFM Policy association
```bash
kubectl get bnksecpolicy.gateway.k8s.f5net.com -n ${app_namespace}
```
