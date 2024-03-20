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
```sh
test
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
```sh
kubectl replace —force -f /tmp/kubectl-edit-temp.yaml
```

---

<p align="center"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>