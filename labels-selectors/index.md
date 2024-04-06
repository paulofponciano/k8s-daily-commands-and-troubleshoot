---
title: Labels / Selectors
layout: default
nav_order: 7
---

# Labels / Selectors

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Get

```sh
kubectl get pods --selector env=dev
```
```sh
kubectl get all --selector env=prod,bu=finance,tier=frontend
```

### Label

```sh
kubectl label nodes node01 size=Large
```

## Examples

- Node Selector:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "1"
    ports:
    - containerPort: 80
  restartPolicy: Always
  nodeSelector:
    env: prd
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)