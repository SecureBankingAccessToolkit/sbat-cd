## Argo Application definitions
This helm chart contains the [Argo Application definitions](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#applications) of our securebanking services. They represent the services inside a kubernetes context and Argo will ensure they are in the desired state defined by our individual service helm charts.

An Application can be either a helm/kustomize chart that lives in git or a helm repository.

### Git
A git application must supply the following:

- `repoURL`: The Git repository URL
- `targetRevision`: Git branch
- `path`: path to the helm chart or kubernetes Manifest definitions.

If the git repository supplies a helm chart then parameters can be overridden. ArgoCD is designed to use the exact chart as declared in Git but you can override parameters which is useful for testing in other environments etc.

### helm
You can supply a helm chart directly from a helm repository, in this case the following definitions are required

- `repoUrl`: The helm repository
- `chart`: The helm chart name
- `targetRevision`: The helm chart version

See the [MongoDB](./templates/mongodb.yaml) Application manifest for an example

## Automated updating
The application definitions allow developers to update images in their environments when they push a new commit to an open PR. The Argo [image-updater](https://argocd-image-updater.readthedocs.io/en/latest/install/strategies/#update-to-the-most-recent-pushed-version-of-a-tag) is always monitoring for new images in our gcr repo. Our Application manifests have a `digest` update strategy which specifically looks for changes to mutable tags (`latest` or `pr-*`). When a change is detected the image-updater will modify the helm parameter `deployment.imageOverride.tag` with the new digest ad argo will take over with the automated update. The image-updater is currently set to poll for a new digest every 2 mins so you can expect some delay when you push a commit.

A digest delimiter must be set in the repo for the correct deployment. deploying a digest takes the form `some/image@sha256:abc...`. The `@` symbol must be handled via the helm chart. Our helm charts default to the form `some/image:tag` but have been made configurable.