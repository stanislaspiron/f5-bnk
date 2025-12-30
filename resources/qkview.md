# Launch qkview creation

```bash
qkviewName=test-issue
```

# Launch qkview creation

```bash
qkviewid=$(./cwc-api.sh v1/qkview -d '{"filename": "'${qkviewName}'"}' | jq -r .id)
```

# Get qkview status

```bash
./cwc-api.sh v1/qkview/${qkviewid}/status
```
# Download qkview

```bash
./cwc-api.sh v1/qkview/${qkviewid}/download -o ${qkviewName}.qkview.tar.gz
```

# List all qkview requests

```bash
./cwc-api.sh v1/qkview
```

# Delete qkview

```bash
./cwc-api.sh v1/qkview/${qkviewid} -X DELETE
```
