# Security / Secrets

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

```sh
kubectl create secret generic \ 
  app-secret --from-literal=DB_Host=mysql \
                      --from-literal=DB_User=root
                      --from-literal=DB_Password=passwd
```
```sh
kubectl create secret generic \
  app-secret --from-file=app_secret.properties
```

### Curl in API Server

```sh
curl -v -k https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt
```

## Examples

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host:
  DB_User:
  DB_Password:
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: firstpod-web
  labels:
    app: firstpod-web
    tier: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80
      envFrom:
        - secretRef:
             name: app-secret
```

---

<p align="center"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>