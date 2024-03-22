---
title: Pods
layout: default
nav_order: 2
---

# Pods

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Get

```sh
kubectl get pods -o wide -A
```
```sh
kubectl get all -A
```
```sh
kubectl get events -o wide
```

### Exec

```sh
kubectl exec POD_NAME -- COMMAND > /LOCALHOST_PATH/file.out
```

### Run
```sh
kubectl run -it debian-pod --image=debian bash
```
```sh
kubectl run static-busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- sleep 1000
```

### Replace

- Forçando o _replace_ com o arquivo temporário após realizar o ```kubectl edit pod```:

```sh
kubectl replace —force -f /tmp/kubectl-edit-42138764059.yaml
```

### Describe

```sh
kubectl describe pod POD_NAME -n NAMESPACE
```
```sh
kubectl describe pod mysql -n app-space | grep -i label
```

### Logs
```sh
kubectl logs POD_NAME -n NAMESPACE
```
```sh
kubectl logs web-app -n frontend
```
```sh
kubectl logs web-app -f -n frontend
```

- Caso o pod tenha sido terminado:

```sh
kubectl logs web-app -f --previous -n frontend
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)