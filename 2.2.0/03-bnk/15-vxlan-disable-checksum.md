# Disable TCP Checksum on VXLAN

TCP offload engine  is a technology used in some network interface cards (NIC) to offload processing of the entire TCP/IP stack to the network controller.

The packet initiator insert a packet checksum before transmiting to the receiver. The receiver validate the checksum before accepting it. If the checksum is wrong, the packet is silently dropped.

In virtual environments, the checksum can be malformed, leading to dropped packets.

This feature can be disabled by setting DISABLE_CHECKSUM_OFFLOAD=true on CSRC pods.

This can be configured with one of following methods:
- Create an entry in CNEInstance manifest **(already done in previous configuration)**
- Use following command :

```bash
kubectl set env  daemonset -n ${f5_utils_namespace} f5-spk-csrc DISABLE_CHECKSUM_OFFLOAD=true
```