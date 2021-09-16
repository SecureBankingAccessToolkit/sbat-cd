# sbat-cd
Manages the continuous deployment of the secure banking application. Uses [ArgoCD](https://argoproj.github.io/argo-cd/) as the continuous deployment platform.

- [argo applications](./argo/apps) - Contains the [Application definitions](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#applications) representing the instance of a service within a kubernetes context. 
