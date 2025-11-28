# Increase init-f5-toda-tmstats memory

## enter maintenance mode
```
kubectl patch bnkgatewayclass.k8s.f5.com my-bnkgatewayclass -n f5-bnk --type=merge -p '{"spec": {"advanced": {"maintenanceMode": {"enabled": true}}}}' 2>&1> /dev/null
 ```
## Change init-f5-toda-tmstats initContainer to 15M
```
export TMSTATS_MEMORY_REQUEST="15Mi"
 ```
## Get container index from list
```
export tmstats_c=$(kubectl get daemonset f5-tmm -n f5-bnk -o json 2> /dev/null | jq '.spec.template.spec.initContainers | map(.name) | index("init-f5-toda-tmstats")')
 ```
## Get container request memory (Run before and after change to compare values)
```
kubectl get daemonset f5-tmm -n f5-bnk -o json | jq -r ".spec.template.spec.initContainers[${tmstats_c}].resources[].memory"
 ```
## Change requests and limits to 15Mi
```
kubectl patch daemonset f5-tmm -n f5-bnk --type=json -p "[ \
    {\"op\": \"replace\",\"path\": \"/spec/template/spec/initContainers/${tmstats_c}/resources/limits/memory\",\"value\": \"${TMSTATS_MEMORY_REQUEST}\"},\
    {\"op\": \"replace\",\"path\": \"/spec/template/spec/initContainers/${tmstats_c}/resources/requests/memory\",\"value\": \"${TMSTATS_MEMORY_REQUEST}\"}\
]" 2>&1> /dev/null
```
