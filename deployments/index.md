---
title: Deployments
layout: default
nav_order: 3
---

# Deployments

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Create

```sh
kubectl create deployment webapp --image=nginx --replicas=3
```

- Dry-Run:

```sh
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

- Dry-Run com saída para yaml:

```sh
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```
```sh
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```

### Rollout

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
kubectl rollout restart deployment.apps/frontend-deployment
```
```sh
kubectl rollout pause deployment.apps/frontend-deployment
```
```sh
kubectl rollout resume deployment.apps/frontend-deployment
```

### Get

```sh
kubectl -n app-space get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name
```

### Scale

```sh
kubectl scale deployment.apps/frontend-deployment --replicas=11
```

### Describe

```sh
kubectl describe deployment.apps/frontend-deployment | grep StrategyType
```

## Examples

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
  replicas: 2
  selector:
    matchLabels:
      name: busybox-pod
  template:
    metadata:
      labels:
        name: busybox-pod
    spec:
      containers:
      - name: busybox-container
        image: busybox
        command:
        - sh
        - "-c"
        - echo Hello Kubernetes! && sleep 3600
```

- Service Account, Env e envFrom:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backstage
  namespace: backstage
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backstage
  template:
    metadata:
      labels:
        app: backstage
    spec:
      serviceAccountName: backstage-service-account
      containers:
        - name: backstage
          image: paulofponciano/backstage:latest
          imagePullPolicy: 'Always'
          resources:
            requests:
              memory: "64Mi"
              cpu: "250m"
            limits:
              memory: "128Mi"
              cpu: "1"
          ports:
            - name: http
              containerPort: 7007
          envFrom:
            - secretRef:
                name: postgres-secrets
          env:
          - name: POSTGRES_PORT
            value: "5432"
          - name: POSTGRES_HOST
            value: "postgres.backstage.svc.cluster.local"    
```

---

<p align="left"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>

![pages branch main](https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot/actions/workflows/ci-gh-pages.yaml/badge.svg?branch=main)