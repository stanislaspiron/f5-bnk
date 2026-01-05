# Firewall Policy

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

# Security Log Profile

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
      publisher: hsl-pub-${f5_bnk_namespace}
      events:
        aclMatchAccept: true
        aclMatchDrop: true
        aclMatchReject: true
        translationFields: true
      format:
        type: user-defined
        userDefinedFieldList: "DateTime:\${date_time},Host:\${bigip_hostname},MgmtIP:\${management_ip_address},FwPolicy:\${acl_policy_name},SrcIP:\${src_ip},SrcPort:\${src_port},DstIP:\${dest_ip},DstPort:\${dest_port},TrSrcIP:\${translated_src_ip},TrDstIP:\${translated_dest_ip},TrSrcPort:\${translated_src_port},TrDstPort:\${translated_dest_port},Protocol:\${protocol},Action:\${action},Context:\${context_name}"
EOF
```

# Assign Security profile and firewall policy to gateway

```bash
cat <<EOF | kubectl apply -f -
---
apiVersion: "gateway.k8s.f5net.com/v1alpha1"
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

# List AFM Policies
```bash
kubectl get f5bigfwpolicy.k8s.f5net.com -n ${app_namespace}
```

# List Log Profile
```bash
kubectl get F5BigLogProfile.k8s.f5net.com -n ${app_namespace}
```

# List AFM Policy association
```bash
kubectl get bnksecpolicy.gateway.k8s.f5net.com -n ${app_namespace}
```


