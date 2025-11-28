# Prometheus


## Create namespace
```bash
kubectl create namespace ${telemetry_namespace}
```

## prometheus certificate

```bash
cat <<EOF | kubectl apply -f -
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: prometheus-client
  namespace: ${telemetry_namespace}
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
      - PD
  emailAddresses:
    - clientcert@f5net.com
  commonName: f5net.com
  secretName: prometheus-client-secret
  issuerRef:
    name: bnk-intermediate-ca
    group: cert-manager.io
    kind: ClusterIssuer
  # Lifetime of the Certificate is 1 hour, not configurable
  duration: 2160h
  privateKey:
    rotationPolicy: Always
    encoding: PKCS1
    algorithm: RSA
    size: 4096
EOF
```
## Prometheus Helm values
```bash
cat <<EOF > prometheus-values.yaml
prometheus-pushgateway:
  enabled: false

prometheus-node-exporter:
  enabled: false 

kube-state-metrics:
  enabled: false 

alertmanager:
  enabled: false 

configmapReload:
  prometheus: 
    enabled: false 
serverFiles:
  prometheus.yml:
    scrape_configs:
      - job_name: bnk-otel
        scheme: https
        static_configs:
          - targets:
              - otel-collector-svc.${f5_bnk_namespace}.svc.cluster.local:9090
        tls_config:
          cert_file: /etc/prometheus/certs/tls.crt
          key_file: /etc/prometheus/certs/tls.key
          ca_file: /etc/prometheus/certs/ca.crt
          insecure_skip_verify: false
server:
  extraVolumes:
    - name: prometheus-tls
      secret:
        secretName: prometheus-client-secret
  extraVolumeMounts:
    - name: prometheus-tls
      mountPath: /etc/prometheus/certs
      readOnly: true
  global:
    scrape_interval: 10s
  service:
    type: "NodePort"
    nodePort: 31929
  configmapReload:
    enabled: false
  persistentVolume:
    enabled: false
EOF
```

## Apply prometheus Helm chart
```bash
helm install prometheus oci://ghcr.io/prometheus-community/charts/prometheus -n ${telemetry_namespace} --atomic -f prometheus-values.yaml
```


# Resources

## Prometheus dashboard

http://node:31929

## f5.virtual_server.clientside.received.bytes
```
f5_tmm_f5_virtual_server_clientside_received_bytes_total 
```
## f5.virtual_server.clientside.connections.count
```
f5_tmm_f5_virtual_server_clientside_connections_count_total 
```
## f5.virtual_server.clientside.connections.current
```
f5_tmm_f5_virtual_server_clientside_connections_current 
```

## f5.pool.member.serverside.connections.count

```
f5_tmm_f5_pool_member_serverside_connections_count_total
```
