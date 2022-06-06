## namespace-setup
This helm chart has the purpose of bringing an SSL certificate from the google secrets manger. We use this to limit our dependency on cert-manager. Our namespaces are created and destroyed frequently and cert-manager has the tendency of requesting new prod letsencrypt certificates which regularly gives us a 429 throttle for cert requests.

By default, the chart will use the dev-crt and dev-key secrets from secret manager, but this may be over-ridden usign the externalCert.certPrefix value (see below).

## Values

| Value    | description                          | default           |
|------------------------------|-----------------------------------------------------------------------|-------------------|
| externalCert.secretName      | Name of the kubernetes secret to store the sslcert                   | `sslcert`         |
| externalCert.projectId       | GCP project that stores the secrets                                   | `example-project` |
| externalCert.certPrefix      | The prefix of the cert to pull from google secrets manager . Use 'dev' to use the "dev" cert |dev|

## Example

The following will create the `bohocode-cdk` namespace, and use the cluster/certificate chart to deploy a secret to sslcert in `bohocode-cdk` namespace. It will deploy the the secrets called `bohocode-crt` and `bohocode-key` from google secret manager to tls.crt and tls.key in the `sslcert` secret. 

```
helm upgrade cdk cluster/certificate --install --namespace bohocode-cdk --create-namespace --set namespace=bohocode-cdk --set externalCert.projectId=sbat-dev --set externalCert.certPrefix=bohocode
```


