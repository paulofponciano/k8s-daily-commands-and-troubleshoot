---
title: Security / Secrets
layout: default
nav_order: 6
---

# Security / Secrets

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

- Secrets:

```sh
kubectl create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
```
```sh
kubectl create secret generic app-secret --from-file=app_secret.properties
```
```sh
kubectl create secret docker-registry regcred \
   --docker-server= registry-name.io  \
   --docker-username= registry-user \
   --docker-password= registry-password \
   --docker-email= registry-user@company.io
```
```sh
kubectl create secret docker-registry private-reg-cred --docker-username=dock_user --docker-password=dock_password --docker-server=myprivateregistry.com:5000 --docker-email=dock_user@myprivateregistry.com
```

- Role:

```sh
kubectl create role developer --resource=pods --verb=create,list,get,update,delete --namespace=development
```

- Role Binding:

```sh
kubectl create rolebinding developer-role-binding --role=developer --user=john --namespace=development
```

- Service Account:

```sh
kubectl create serviceaccount dashboard-sa
```

- Token (Com tempo de expiração):

```sh
kubectl create token [SERVICEACCOUNT_NAME]
```

```sh
kubectl create token dashboard-sa
```

- Code / Decode para base 64:

```sh
echo -n 'value' | base64
```
```sh
echo 'value' | base64 --decode 
```

### Get

- CertificateSigningRequest:

```sh
kubectl get csr akshay -o yaml
```

### Describe

```sh
kubectl describe secret dashboard-sa-token
```

### Certificate

- CertificateSigningRequest:

```sh
kubectl certificate approve akshay
```
```sh
kubectl certificate deny agent-smith
```

### Delete

- CertificateSigningRequest:

```sh
kubectl delete csr agent-smith
```

### Auth

- Checando permissões:

```sh
kubectl auth can-i create deployments
```
```sh
kubectl auth can-i delete nodes
```
```sh
kubectl auth can-i delete nodes --as user-1
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

- Secret:

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

- envFrom:

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

- valueFrom:

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
      env:
      - name: DB_Password
        valueFrom:
          secretKeyRef:
            name: app-secret
            key: DB_Password
```

- Volume:

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
      volumeMounts:
        - name: app-secret-volume
          readOnly: true
          mountPath: "/etc/app-secret-volume"
  volumes:
    - name: app-secret-volume
      secret:
        secretName: app-secret
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
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
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
      runAsUser: 1000
      capabilities:
        add: ["SYS_TIME"]
  restartPolicy: Always
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  securityContext:
    runAsUser: 1001
  containers:
  -  image: ubuntu
     name: web
     command: ["sleep", "5000"]
     securityContext:
      runAsUser: 1002

  -  image: ubuntu
     name: sidecar
     command: ["sleep", "5000"]
```

- Service Account:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard-sa
```

- Secret para Service Account (Token):

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: dashboard-secret
  annotations:
    kubernetes.io/service-account.name: dashboard-sa
```

- Network policy:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy-ingress
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:  #primeira regra
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod
    - ipBlock:      #segunda regra
        cidr: 10.10.5.20/32
    ports:
    - protocol: TCP
      port: 3306
```
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy-ingress-egress
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:  #primeira regra
        matchLabels:
          name: api-pod
      namespaceSelector:
        matchLabels:
          name: prod
    - ipBlock:      #segunda regra
        cidr: 10.10.5.20/32
    ports:
    - protocol: TCP
      port: 3306
  egress:
  - to:
    - ipBlock:
        cidr: 10.10.5.20/32
    ports: 
    - protocol: TCP
      port: 80
```
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```

- CertificateSigningRequest:

```sh
cat akshay.csr | base64 -w 0
```

```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: <Paste the base64 encoded value of the CSR file>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```

- Approve:

```sh
kubectl certificate approve akshay
```

- Registry secret - ```imagePullSecrets```:

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
      image: registry-name.io/nginx-custom-image
      ports:
        - containerPort: 80
  imagePullSecrets:
    - name: regcred
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)