

# Deploy BNK Gateway Class
The BIG-IP Next for Kubernetes is deployed through the application of the BnkGatewayClass CR, which allows users to specify the desired state of the BIG-IP Next for Kubernetes cluster. The F5 Lifecycle Operator (FLO) utilizes this BnkGatewayClass CR as an input file to instantiate the BIG-IP Next for Kubernetes component CRs, which deploy the necessary BIG-IP Next for Kubernetes pods with predefined configurations. At the same time, the IPAM Operator utilizes the IPAM Controller CR, created by FLO, as an input file to deploy the IPAM Controller. 

The networkAttachments section lists attached networkAttachments to the tmm pods.  In the example:
  - **external-net** networkAttachment will be bind as interface **1.1**
  - **internal-net** networkAttachment will be bind as interface **1.2**

```bash
cat <<EOF | kubectl apply -f -
apiVersion: k8s.f5.com/v1
kind: BNKGatewayClass
metadata:
  labels:
    app.kubernetes.io/name: f5-lifecycle-operator
  name: lab-bnk-gw
  namespace: ${f5_bnk_namespace}
spec:
  manifestVersion: ${f5_bnk_version}
  containerPlatform: Generic
  # Disable telemetry
  telemetry:
    loggingSubsystem:
      enabled: false
    metricSubsystem:
      enabled: true
  certificate:
    clusterIssuer: bnk-intermediate-ca
  deploymentSize: "Small"
  image:
    repository: "repo.f5.com/images"
    imagePullSecrets:
    - name: far-secret
    imagePullPolicy: Always
  networkAttachments:
  - external-net
  - internal-net

  # Features
  # CSRC Egress
  pseudoCNI:
    enabled: true
  # BGP: Disable
  dynamicRouting:
    enabled: false
  # Core dump files
  coreCollection:
    enabled: false
  # AFM: Disable
  firewallACL:
    enabled: true

  advanced:
    envDiscovery:
    # Disable for Demo Mode
      enabled: false
      stopOnFail: true
      runAfterSuccess: true
    cneController:
      env:
      - name: "TMM_DEFAULT_MTU"
        value: "9000"

    demoMode:
      enabled: true
    maintenanceMode:
      enabled: false
    pseudoCNI:
      env:
      - name: "EXCLUDE_CIDR"
        # K3S POD CIDR = 10.42.0.0/16
        # K3S Services CIDR = 10.43.0.0/16
        value: "${pod_network},${service_network}"
      # Below is specific to VM environments only : disable VXLAN checksum - does not work with pod and tmm on same node
      - name: "DISABLE_CHECKSUM_OFFLOAD"
        value: "true"
    tmm:
      env:
      - name: "TMM_CALICO_ROUTER"
        value: "default"
      - name: TMM_DEFAULT_MTU
        value: "9000"
      # Below is specific to VM environments only : support VETH interfaces on Data Plan
      - name: "TMM_MAPRES_ADDL_VETHS_ON_DP"
        value: "true"
EOF
```


[Back](09-flo.md)  
[Next](11-gatewaclass.md)