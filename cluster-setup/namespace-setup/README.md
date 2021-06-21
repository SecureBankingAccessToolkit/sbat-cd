## namespace-setup
This helm chart has the purpose of bringing an SSL certificate from the google secrets manger. We use this to limit our dependency on cert-manager. Our namespaces are created and destroyed frequently and cert-manager has the tendency of requesting new prod letsencrypt certificates which regularly gives us a 429 throttle for cert requests.

## Values

| Value | description | default |
| ----- | ----------- | ------- |
| externalCert.enabled | enable wildcard certificate | `true` |
| externalCert.secretName | Name of the kubernetes secret to store the wildcard | `sslcert` |
| externalCert.projectId | GCP project that stores the secrets | `example-project` |
