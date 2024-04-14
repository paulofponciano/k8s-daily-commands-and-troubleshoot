
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
kubectl get pods -A -o custom-columns=NAME:.metadata.name,CONTAINER_IMAGE:.spec.containers[].image,NAMESPACE:.metadata.namespace,STATUS:.status.phase,NODE:.spec.nodeName --sort-by=.metadata.name
```

### Exec

```sh
kubectl exec POD_NAME -- COMMAND > /LOCALHOST_PATH/file.out
```

### Run

```sh
kubectl run -it debian-pod --image=debian bash
```

- Dry-Run:

```sh
kubectl run static-busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- sleep 1000
```

- Dry-Run com saída para yaml:

```sh
kubectl run static-busybox --image=busybox --restart=Never --dry-run=client -o yaml > manifest.yaml
```

### Static pods config (worker - kubelet):

```sh
cd /etc/kubernetes/manifests
```
```sh
/etc/kubernetes/manifests/static-pod.yaml
```

- kubelet.service:

```sh
ExecStart=/usr/local/bin/kubelet \\
  ...
  --pod-manifest-path=/etc/kubernetes/manifests \\
  ...
```

ou

```sh
ExecStart=/usr/local/bin/kubelet \\
  ...
  --config=kubeconfig.yaml \\
  ...
```

- kubeconfig.yaml:

```sh
staticPodPath: /etc/kubernetes/manifests
```

- Identificar path dos manifestos estáticos:

```sh
ps -aux | grep kubelet
```
  - Saida do ps:
```sh
...
--config=/var/lib/kubelet/config.yaml
...
```
  - Path no arquivo config.yaml
```sh
staticPodPath: /etc/kubernetes/manifests
```

- Criar static pod:

```sh
kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
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
kubectl logs -f POD_NAME CONTAINER_NAME -n NAMESPACE
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

### Top

```sh
kubectl top pod
```
```sh
kubectl top pod -A --sort-by='memory'
```
```sh
kubectl top pod -A --sort-by='memory' --no-headers | head -1
```
```sh
kubectl top pod -A --sort-by='cpu'
```
```sh
kubectl top pod -A --sort-by='cpu' --no-headers | tail -1
```

## Examples

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
```

- Schedule manual:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: node01
  containers:
  -  image: nginx
     name: nginx
```

- Definir scheduler:

```yaml
---
apiVersion: v1 
kind: Pod 
metadata:
  name: nginx 
spec:
  schedulerName: my-custom-scheduler
  containers:
  - image: nginx
    name: nginx
```

- Comandos e argumentos:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-admin
  namespace: default
spec:
  containers:
  - image: ubuntu
    command:
      - "/bin/bash"
      - "-c"
      - >
        apt-get update &&
        apt-get install -y vim curl git unzip iputils-ping telnet &&
        curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" &&
        chmod +x ./kubectl &&
        mv ./kubectl /usr/local/bin/kubectl &&
        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" &&
        unzip awscliv2.zip &&
        ./aws/install &&
        sleep 604800
    imagePullPolicy: IfNotPresent
    name: ubuntu
  restartPolicy: Always
```

```yaml
apiVersion: v1
kind: Pod 
metadata:
  name: ubuntu-sleeper-3
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
      - "sleep"
      - "1200"
```

```yaml
apiVersion: v1 
kind: Pod 
metadata:
  name: webapp-green
  labels:
      name: webapp-green 
spec:
  containers:
  - name: simple-webapp
    image: kodekloud/webapp-color
    command: ["python", "app.py"]
    args: ["--color", "pink"]
```

> [!NOTE]
> No manifesto, o 'command' sobrepõe o 'entrypoint' do Dockerfile e 'args' sobrepõe 'CMD' do Dockerfile.

```dockerfile
FROM python:3.6-alpine

RUN pip install flask

COPY . /opt/

EXPOSE 8080

WORKDIR /opt

ENTRYPOINT ["python", "app.py"]

CMD ["--color", "red"]
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)