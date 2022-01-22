# sealedsecret-deployment
## Instalacion
```shell
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm install --namespace kube-system sealed-secrets-controller sealed-secrets/sealed-secrets
```