# Connect to tmm pod

```bash
kubectl exec -it daemonset/f5-tmm -c debug -n f5-bnk -- bash
```

# View configuration

## Show virtual servers
```
configview virtual_server
```

result

```
request:[declTmm.virtual_server]:{id:"myapp-vip12-10.245.3.12-https-vs"  enabled:true  source_address_translation_type:"SRC_TRANS_AUTOMAP"  default_pool:"myapp-vip12-10.245.3.12-https-empty-pool-gwroute"  traffic_matching_criteria:"myapp-vip12-10.245.3.12-https-tmc"  irules_reference:"myapp-irule-ping"} action:0 state:5 
```

## Show all configuration

```
configview show all
```

# View virtual servers (including egress)

```
tmctl -d blade virtual_server_stat -s name
```

# View virtual servers statistics (including egress)

```
tmctl -w 300 -d blade virtual_server_stat -s name,clientside.pkts_in,serverside.pkts_out,clientside.tot_conns,serverside.tot_conns
```

# Firewall usage

```
tmctl -w 300 -d blade fw_hw_offload_stats
```
# View VLANs

```
tmctl -d blade ifc_stats --select=name,type
```

# View routes

```
bdt_cli route
```

results
```
routeType:0 isIpv6:false destNet:{ip:{addr:10.233.102.187, rd:0} pl:32} gw:{ip:{addr:10.245.1.251, rd:0}} gwType:1 interface:internal unresolved:false 
routeType:0 isIpv6:false destNet:{ip:{addr:10.233.0.0, rd:0} pl:18} gw:{ip:{addr:169.254.1.1, rd:0}} gwType:1 interface:eth0 unresolved:false 
routeType:0 isIpv6:false destNet:{ip:{addr:10.233.64.0, rd:0} pl:18} gw:{ip:{addr:169.254.1.1, rd:0}} gwType:1 interface:eth0 unresolved:false 
routeType:1 isIpv6:false destNet:{ip:{addr:10.245.1.0, rd:0} pl:24} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:internal unresolved:false 
routeType:1 isIpv6:false destNet:{ip:{addr:10.245.2.0, rd:0} pl:24} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:external unresolved:false 
routeType:0 isIpv6:false destNet:{ip:{addr:169.254.1.1, rd:0} pl:32} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:eth0 unresolved:false 
routeType:1 isIpv6:false destNet:{ip:{addr:10.233.71.59, rd:0} pl:32} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:eth0 unresolved:false 
routeType:1 isIpv6:false destNet:{ip:{addr:169.254.0.0, rd:0} pl:24} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:tmm unresolved:false 
routeType:0 isIpv6:true destNet:{ip:{addr:<none>, rd:0} pl:0} gw:{ip:{addr:fe80::ecee:eeff:feee:eeee, rd:0}} gwType:1 interface:eth0 unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:fe80::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:internal unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:ff02::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:internal unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:fe80::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:external unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:ff02::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:external unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:fe80::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:tmm unresolved:false 
routeType:0 isIpv6:true destNet:{ip:{addr:fe80::ecee:eeff:feee:eeee, rd:0} pl:128} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:eth0 unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:fe80::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:eth0 unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:ff02::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:eth0 unresolved:false 
routeType:1 isIpv6:true destNet:{ip:{addr:ff02::, rd:0} pl:64} gw:{ip:{addr:<none>, rd:0}} gwType:0 interface:tmm unresolved:false 
```

# iRules usage

```
tmctl -d blade rule_stat -s name,event_type,failures,total_executions
```

# Firewall rules usage

```
tmctl -d blade fw_rule_stat
```

# Client-ssl usage

```
tmctl -d blade profile_clientssl_stat
```