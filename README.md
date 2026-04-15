# ArgoCD Demo

This repository contains a simple ArgoCD demo with a root application and a child application for deploying an Nginx server.

# ArgoCD installation:
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \
  --version 7.8.23 \
  --set server.service.type=ClusterIP
```

## Check ArgoCD server is running:
```
kubectl get pods -n argocd
```

### Access ArgoCD UI:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Then open http://localhost:8080 in your browser.


### Retrieve ArgoCD initial admin password:
```
kubectl get secret argocd-initial-admin-secret \
  -n argocd \
  -o jsonpath="{.data.password}" | base64 -d && echo
```

### Login with CLI
```
argocd login localhost:8080 \
  --username admin \
  --password <password> \
  --insecure
```

### Change initial admin password:
```
argocd account update-password \
  --current-password <oldpassword> \
  --new-password <new-strong-password>

kubectl delete secret argocd-initial-admin-secret -n argocd
```

## ArgoCD image Updater installation:

```
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/manifests/install.yaml
```