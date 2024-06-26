---
title: Namespaces
layout: default
nav_order: 11
---

# Namespaces

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Alterar contexto namespace default

```sh
kubectl config set-context $(kubectl config current-context) --namespace=NAMESPACE_NAME
```

## Examples

### Resource Quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "20"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "8"
    limits.memory: 10Gi
```

### Labels

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: backstage
  labels:
    istio-injection: enabled
```

### Limit Range

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default:
      cpu: 500m #limit
    defaultRequest:
      cpu: 500m #request
    max:
      cpu: "1" #limit
    min:
      cpu: 100m #request
    type: Container
```
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: memory-resource-constraint
spec:
  limits:
  - default:
      memory: 1Gi #limit
    defaultRequest:
      memory: 1Gi #request
    max:
      memory: 1Gi #limit
    min:
      memory: 500Mi #request
    type: Container
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)