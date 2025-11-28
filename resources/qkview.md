# Launch qkview creation

```bash
qkviewid=$(./cwc-api.sh v1/qkview -d '{"filename": "calico-issue"}' | jq -r .id)
```

# Get qkview status

```bash
./cwc-api.sh v1/qkview/${qkviewid}/status
```
# Download qkview

```bash
./cwc-api.sh v1/qkview/${qkviewid}/download -o calico-issue.qkview.tar.gz
```

# Delete qkview

```bash
./cwc-api.sh v1/qkview/${qkviewid} -X DELETE
```
