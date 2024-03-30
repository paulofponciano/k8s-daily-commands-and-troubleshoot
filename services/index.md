---
title: Services
layout: default
nav_order: 4
---

# Services

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Describe

```sh
kubectl describe svc SERVICE_NAME -n NAMESPACE
```
```sh
kubectl describe svc mysql-service -n app-space | grep -i selector
```

### Expose

```sh
kubectl expose pod redis --port=6379 --name redis-service
```

## Examples

- ClusterIP:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backstage
  namespace: backstage
spec:
  selector:
    app: backstage
  ports:
    - targetPort: 80
      port: 80
```

- NodePort:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service-webapp
  namespace: app-space
spec:
  selector:
    app: webapp
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  type: NodePort
```

<p align="left"><img src="./img/k8s-services-nodeport.png" width="150" alt="k8s-services-nodeport"></p>

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)