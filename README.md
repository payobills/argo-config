# payobills-argo-config

Argo deployment for payobills.

Depends on [kube-homelab](https://github.com/mrsauravsahu/kube-homelab)'s Argo CD

```bash
alias k=kubectl
k apply -n argo-cd -f 'payobills-argo-config.yaml'
```
