
# Maintenance

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Drain

```sh
kubectl drain node01 --ignore-daemonsets
```
```sh
kubectl drain node01
```

### Cordon / Uncordon

```sh
kubectl cordon node01
```
```sh
kubectl uncordon node01
```

### Delete Evicted Pods

```sh
kubectl get pod -n <namespace> | grep Evicted | awk '{print $1}' | xargs kubectl delete pod -n <namespace>
```

### Patch resources for termination

 ```sh
 kubectl patch <resource>/<resource_name> -n <namespace>  --type json     --patch='[ { "op": "remove", "path": "/metadata/finalizers" } ]'
 ```

### Delete Pods stuck on terminating

```sh
kubectl delete pod <pod> -n <namespace> --grace-period=0 --force

#multiples pods in same namespace
for p in $(kubectl get pods -n <namespace> | grep Terminating | awk '{print $1}'); do kubectl delete pod $p --grace-period=0 --force;done
```

### Cluster upgrade with kubeadm (1.29)

- Control Plane:

```sh
vim /etc/apt/sources.list.d/kubernetes.list
```
```sh
deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
```
```sh
kubectl drain controlplane --ignore-daemonsets
```
```sh
apt update
```
```sh
apt-cache madison kubeadm
```
```sh
apt-get install kubeadm=1.29.0-1.1
```
```sh
kubeadm upgrade plan v1.29.0
```
```sh
kubeadm upgrade apply v1.29.0
```
```sh
apt-get install kubelet=1.29.0-1.1
```
```sh
systemctl daemon-reload
```
```sh
systemctl restart kubelet
```
```sh
kubectl uncordon controlplane
```

- Workers:

```sh
kubectl drain node01 --ignore-daemonsets
```
```sh
vim /etc/apt/sources.list.d/kubernetes.list
```
```sh
deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
```
```sh
apt update
```
```sh
apt-cache madison kubeadm
```
```sh
apt-get install kubeadm=1.29.0-1.1
```
```sh
kubeadm upgrade node
```
```sh
apt-get install kubelet=1.29.0-1.1
```
```sh
systemctl daemon-reload
```
```sh
systemctl restart kubelet
```
```sh
kubectl uncordon node01
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)