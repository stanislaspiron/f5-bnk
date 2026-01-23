# VMWare requirements
When configuring Multus MAC VLAN on VMware ESX or ESXi servers, you must set the virtual switch's Forged Transmits and Promiscuous Mode settings to Accept. (These settings are disabled by default). 

For information about enabling Promiscuous Mode and Forged Transmits on the virtual switch, refer to the VMware knowledge base article listed in the Supplemental section or in the VMware documentation for your specific VMware version. F5 recommends that hypervisor administrators be very conservative with regard to interface usage after you enable promiscuous mode. 

All packets are mirrored to all interfaces in the same portgroup or vSwitch on which promiscuous mode is enabled. 

For each interface in the vSwitch or portgroup, an additional hypervisor CPU is required to copy these packets. This can lead to CPU exhaustion for the hypervisor, even if an interface is uninitialized on the BIG-IP system. F5 recommends that you use only one interface in a portgroup or vSwitch on which promiscuous mode is enabled. 

Starting from VMware ESXI 6.7, Promiscuous Mode can be replaced by MAC Learning in a supported environment, that is, Promiscuous Mode can be set to Reject when MAC Learning is enabled on the vSwitch on which BIGIP's VM is part of that network. The MAC Learning feature is supported only on Distributed Virtual (DV) Port groups.