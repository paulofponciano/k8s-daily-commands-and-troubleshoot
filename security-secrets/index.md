---
title: Security / Secrets
layout: default
nav_order: 6
---

# Security / Secrets

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

- Secrets:

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

- Role:

```sh
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development
```

- Role Binding:

```sh
kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```

### Curl in API Server

```sh
curl -v -k https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods -u “user:password”
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods --header “Authorization: Baerer TOKEN”
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods -insecure --header “Authorization: Baerer TOKEN”
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

- Utilizando ```securityContext```:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pod-with-securitycontext
  name: pod-with-securitycontext
spec:
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox:1.28
    name: with-securitycontext
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
  restartPolicy: Always
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)