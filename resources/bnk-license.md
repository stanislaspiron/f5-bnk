# CWC tool

## Tool requirements

This tool requires CWC pod up and running. To validate CWC status, execute following command:

```bash
kubectl get pod -n f5-utils -l app=cwc
```

Expected result:

```
NAME                         READY   STATUS    RESTARTS      AGE
f5-spk-cwc-cd7b64b4b-hmkzn   2/2     Running   4 (36m ago)   23h
```

## Prepare 
```bash
mkdir -p $HOME/cwc/cwc_api
```
Copy each of the certificates into the new directory (see Create CWC REST API certificates section in Create Cluster Wide Controller Certificates):

```bash
cp api-server-secrets/ssl/client/certs/client_certificate.pem $HOME/cwc/cwc_api
cp api-server-secrets/ssl/ca/certs/ca_certificate.pem $HOME/cwc/cwc_api
cp api-server-secrets/ssl/client/secrets/client_key.pem $HOME/cwc/cwc_api
```

Create a bash file named **cwc-api.sh** with the below code.

```bash
CWC_CERT_PATH="${CWC_CERTS:-$HOME/cwc/cwc_api}"

NODE=$(kubectl -n f5-utils get pod -l app=cwc -o jsonpath='{.items[0].status.hostIP}')
AUTHTOKEN="$(kubectl get secret cwc-auth-token -n f5-utils -o jsonpath="{.data.token}" | base64 -d)"

CURL_CMD=(
   curl --noproxy '*' \
      --cacert "$CWC_CERT_PATH"/ca_certificate.pem \
      --cert "$CWC_CERT_PATH"/client_certificate.pem \
      --key "$CWC_CERT_PATH"/client_key.pem \
      -H "Authorization: Bearer $AUTHTOKEN" \
      --resolve f5-spk-cwc.f5-utils:30881:"$NODE"
)

"${CURL_CMD[@]}" https://f5-spk-cwc.f5-utils:30881/"$@"
```

change file permission

```bash
chmod +x cwc-api.sh
```

## Usage

### Get status
```bash
./cwc-api.sh status | jq
```



# Disconnected mode
With this mode, cwc-api.sh script must bypass proxy, but curl commands to F5 must use proxy.
If the cwc-api.sh script is not installed, 

## Create report (like create dossier in TMOS)

```bash
./cwc-api.sh report > report.txt
```
## get digitalassetid from secrets

```bash
digitalassetid=$(kubectl get secrets -n f5-utils digitalassetid -o json | jq -r .data.digitalassetid | base64 -d)
```
 
## Post report to F5 License server

```bash
curl -X POST https://product.apis.f5.com/ee/v1/entitlements/telemetry \
-H "Content-Type: application/json" -H "F5-DigitalAssetId: ${digitalassetid}" \
-H "User-Agent: CNF" \
-H "Authorization: Bearer ${f5_licenseJWT}" \
-d @report.txt | jq -r .manifest > lic-manifest.txt
```
 
## Apply the License to CWC

```bash
./cwc-api.sh receipt -d @lic-manifest.txt
```

## Check the license status

```bash
./cwc-api.sh status | jq
```


# Reactivate license
In case of reactivation required, use following command
```bash
./cwc-api.sh reactivate -d <licensetoken>
```

# Alternate solution

```bash
kubectl get secret cwc-license-client-certs -n f5-utils -o jsonpath="{.data.ca-root-cert}" | base64 -d > $HOME/cwc/cwc_api/ca_certificate.pem
kubectl get secret cwc-license-client-certs -n f5-utils -o jsonpath="{.data.client-cert}" | base64 -d > $HOME/cwc/cwc_api/client_certificate.pem
kubectl get secret cwc-license-client-certs -n f5-utils -o jsonpath="{.data.client-key}" | base64 -d > $HOME/cwc/cwc_api/client_key.pem
```

# Reset license

```bash
kubectl delete secret -n $f5_utils_namespace activationmessage
kubectl delete secret -n $f5_utils_namespace activationstatus
kubectl delete secret -n $f5_utils_namespace activationtimestamp
kubectl delete secret -n $f5_utils_namespace configreport
kubectl delete secret -n $f5_utils_namespace configreportsignedackresponse
kubectl delete secret -n $f5_utils_namespace context
kubectl delete secret -n $f5_utils_namespace csr
kubectl delete secret -n $f5_utils_namespace customerprovidedid
kubectl delete secret -n $f5_utils_namespace digitalassetid
kubectl delete secret -n $f5_utils_namespace digitalassetname
kubectl delete secret -n $f5_utils_namespace digitalassetversion
kubectl delete secret -n $f5_utils_namespace entitlements
kubectl delete secret -n $f5_utils_namespace initialregistrationstatus
kubectl delete secret -n $f5_utils_namespace licensekey
kubectl delete secret -n $f5_utils_namespace licensestatus
kubectl delete secret -n $f5_utils_namespace modeofoperation
kubectl delete secret -n $f5_utils_namespace previousreportname
kubectl delete secret -n $f5_utils_namespace previousreportverifieddate
kubectl delete secret -n $f5_utils_namespace productname
kubectl delete secret -n $f5_utils_namespace statehistory
kubectl delete secret -n $f5_utils_namespace statesofdecay
kubectl delete secret -n $f5_utils_namespace switchlicensestatus
kubectl delete secret -n $f5_utils_namespace telemetrystatus
```