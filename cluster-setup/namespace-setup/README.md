## namespace-setup
This helm chart is intended to configure a namespace for use with a private docker registry. It will update the default ServiceAccount with the required imagePullSecret. It is useful for dynamically created namespaces that are regularly created and destroyed.

It deploys an empty gcr secret to your namespace with an annotation that is used specifically with the [secrets-replicator](https://github.com/mittwald/kubernetes-replicator). The `imageRepoSecret` is then replicated from its namespace to the namespace you are creating, the default service account will then be updated by the service account update [job](./templates/service-account-update.yaml).

## Reasoning
We are deploying [forgeops](https://github.com/forgerock/forgeops) to our own cluster to increase cycle times. We need to be able to create an destroy this in its own namespace on demand and recreate every day with a new kubernetes cluster. We need access to our own private docker registry for test purposes which is separate from our release repository. This gives us a quick and reliable solution that allows us to pull from a private repo without having to specifically add a service account to each deployment whilst giving us the flexibility to create new kubernetes service accounts with other embedded secrets should we need them.

## Values

| Value | description | default |
| ----- | ----------- | ------- |
| serviceAccount | service account to update with the imagePullSecret | default |
| imageRepoSecret.name | name of the docker image repository secret | |
| imageRepoSecret.namespace | namespace of the docker image repository secret | default |