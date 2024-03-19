# Deployment commands

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

```sh
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```
```sh
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```
```sh
kubectl create deployment webapp --image=nginx --replicas=3
```
```sh
kubectl rollout status deployment.apps/frontend-deployment
```
```sh
kubectl rollout history deployment.apps/frontend-deployment
```
```sh
kubectl rollout history deployment.apps/frontend-deployment --revision=3
```
```sh
kubectl rollout undo deployment.apps/frontend-deployment
```
```sh
kubectl rollout undo deployment.apps/frontend-deployment --to-revision=3
```
```sh
kubectl rollout pause deployment.apps/frontend-deployment
```
```sh
kubectl rollout resume deployment.apps/frontend-deployment
```
```sh
kubectl scale deployment.apps/frontend-deployment --replicas=11
```
```sh
kubectl describe deployment.apps/frontend-deployment | grep StrategyType
```

---

<p align="center"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>