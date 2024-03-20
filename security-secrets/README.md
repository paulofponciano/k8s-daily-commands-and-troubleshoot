# Security / Secrets

<p align="left"><img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" width="80" alt="kube_logo"></p>

## Commands

### Curl in API Server

```sh
curl -v -k https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt
```

---

<p align="center"><a href="https://github.com/paulofponciano/k8s-daily-commands-and-troubleshoot"><img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/paulofponciano/k8s-daily-commands-and-troubleshoot?label=k8s-daily-commands-and-troubleshoot&style=social"></a></p>