# Disable VXLAN checksum offload

Use this command to patch if it was not applied in the BNKGatewayClass manifest

```bash
kubectl set env  daemonset -n f5-utils f5-spk-csrc DISABLE_CHECKSUM_OFFLOAD=true
```