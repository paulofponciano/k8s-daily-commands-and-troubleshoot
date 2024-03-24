
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

### Workers

- Status de serviços:

```sh
systemctl status containerd
```
```sh
service kubelet status
```
```sh
sudo journalctl -u kubelet
```
```sh
ps -aux | grep kubelet | grep --color container-runtime-endpoint
```

- Kubelet config path:

```sh
vim /var/lib/kubelet/config.yaml
```
```sh
vim /etc/kubernetes/kubelet.conf
```

- Informações de certificado no worker:

```sh
openssl x509 -in /var/lib/kubelet/worker2.crt -text
```

### Control Plane

```sh
sudo journalctl -u kube-apiserver
```
```sh
netstat -nplt 
```
```sh
netstat -anp | grep etcd | grep 2379 | wc -l
```
```sh
crictl ps -a | grep kube-apiserver
```
```sh
crictl logs --tail=2 CONTAINER_ID
```

- Manifestos dos componentes de control plane:

```sh
ls /etc/kubernetes/manifests/
```

- Cluster IP range (Control Plane):

```sh
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep cluster-ip-range
```

- Weave net CNI:

```sh
echo 1 > /proc/sys/net/ipv4/ip_forward
```
```sh
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

- IP allocation range (Weave net CNI):

```sh
kubectl logs weave-net-6cm2b weave -n kube-system | grep ipalloc-range
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)