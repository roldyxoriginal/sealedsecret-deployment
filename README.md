# Instalación de Sealed Secret

## Instalación de kubeseal - Herramienta necesaria en el cliente para poder crear sealed secret

```
wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.17.4/kubeseal-0.17.4-linux-amd64.tar.gz
tar xvfz kubeseal-0.17.4-linux-amd64.tar.gz kubeseal 
sudo install -m 755 kubeseal /usr/local/bin/kubeseal
```
## Instalación del producto
```
#Instalamos usando Helm (Sin internet)
helm install sealed-secrets-controller ./sealed-secrets -n kube-system
#### Para instalar el producto desde internet
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm install sealed-secrets-controller sealed-secrets/sealed-secrets -n kube-system --version 2.1.5

```
## Creando un Sealed Secret

#### Creamos un secret en modo dry-run para que no genere el secret en kubernetes, luego lo ciframos con la clave publica de sealed
```
kubectl create secret generic secret-name --dry-run --from-literal=foo=bar -o yaml |  kubeseal --format yaml > mysealedsecret.yaml
```
#### Para obtener la clave publica de sealed secret
```
kubeseal --fetch-cert > mycert.pem
```
#### Finalmente creamos el secret
```
kubectl create -f mysealedsecret.yaml
```
#### Vemos que se creo el secret con los datos publicados
```
kubectl get secret secret-name -o yaml
apiVersion: v1
data:
  foo: YmFy <-- aca esta el valor en base 64

# Obtención del valor
echo YmFy|base64 -d
bar
```
#### Para obtener ayuda
helm get notes sealed-secrets -n sealed-secret
