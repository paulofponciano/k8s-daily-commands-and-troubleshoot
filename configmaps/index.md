---
title: ConfigMaps
layout: default
nav_order: 13
---

# ConfigMaps

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

```sh
kubectl create configmap \
    <config-name> --from-literal=<key>=<value>
```
```sh
kubectl create configmap \
    app-config --from-literal=APP_COLOR=blue
```
```sh
kubectl create configmap \
    <config-name> --from-file=<path-to-file>
```
```sh
kubectl create configmap \
    app-config --from-file=app_config.properties
```

## Examples

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
     ports:
     - containerPort: 80
     envFrom:
     - configMapRef:
         name: app-config
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)