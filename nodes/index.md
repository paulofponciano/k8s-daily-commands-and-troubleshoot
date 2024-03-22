---
title: Nodes
layout: default
nav_order: 10
---

# Nodes

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Get

```sh
kubectl get nodes
```
```sh
kubectl get nodes -o wide
```
```sh
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
```
```sh
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
```
```sh
kubectl get nodes -o=jsonpath='{.items[*].status.capacity.cpu}'
```
```sh
kubectl get nodes -o=jsonpath='{.items[*].metadata.name} {"\n"} {.items[*].status.capacity.cpu}'
```
```sh
kubectl get nodes -o=jsonpath='{range .items[*]} {.metadata.name} {"\t"} {.status.capacity.cpu} {"\n"} {end}'
```
```sh
kubectl get nodes -o=custom-columns=NODE: .metadata.name, CPU: .status.capacity.cpu
```
```sh
kubectl get nodes --sort-by= .metadata.name
```
```sh
kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}'
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)