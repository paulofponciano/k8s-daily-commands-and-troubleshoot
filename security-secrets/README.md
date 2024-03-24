
# Security / Secrets

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

- Secrets:

```sh
kubectl create secret generic \ 
  app-secret --from-literal=DB_Host=mysql \
                      --from-literal=DB_User=root
                      --from-literal=DB_Password=passwd
```
```sh
kubectl create secret generic \
  app-secret --from-file=app_secret.properties
```

- Role:

```sh
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development
```

- Role Binding:

```sh
kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```

- Code / Decode para base 64:

```sh
echo -n 'value' | base64
```
```sh
echo 'value' | base64 --decode 
```

### Curl in API Server

```sh
curl -v -k https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods -u “user:password”
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods --header “Authorization: Baerer TOKEN”
```
```sh
curl -v -k https://master-node-ip:6443/api/v1/pods -insecure --header “Authorization: Baerer TOKEN”
```

## Examples

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host:
  DB_User:
  DB_Password:
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: firstpod-web
  labels:
    app: firstpod-web
    tier: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80
      envFrom:
        - secretRef:
             name: app-secret
```

- Role:

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "create","delete"]
```
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
  namespace: blue
rules:
- apiGroups:
  - ""
  resourceNames:
  - blue-app
  - dark-blue-app
  resources:
  - pods
  verbs:
  - get
  - watch
  - create
  - delete
```

- Role Binding:

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dev-user-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

- Cluster Role:

```yaml
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: node-admin
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "watch", "list", "create", "delete"]
```

- Cluster Role Binding:

```yaml
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: michelle-binding
subjects:
- kind: User
  name: michelle
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: node-admin
  apiGroup: rbac.authorization.k8s.io
```

- Utilizando ```securityContext```:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pod-with-securitycontext
  name: pod-with-securitycontext
spec:
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox:1.28
    name: with-securitycontext
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
  restartPolicy: Always
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)