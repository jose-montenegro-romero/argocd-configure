# Agregar el repo oficial
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

# Ver las versiones de argocd
helm search repo argo/argo-cd --versions

# Descarga una versión específica
helm pull argo/argo-cd --version 9.5.14 --untar

# Para cambiar el pass por defecto buscar en values.yaml la propiedad argocdServerAdminPassword
htpasswd -nbBC 10 "" ARGOCDPASS | tr -d ':\n' | sed 's/$2y/$2a/'


# Instalar ArgoCD Custom values
helm install argocd argo-cd/ \
  --namespace argocd \
  --create-namespace --wait \
  -f argo-cd/values.yaml

helm upgrade argocd argo-cd/ \
  --namespace argocd \
  --wait \
  -f argo-cd/values.yaml

# Ver status del helm
helm list -n argocd

# Exponer Argo CD en localhost
kubectl port-forward svc/argocd-server -n argocd 8080:443

# Entrar con el usuario:

admin/ARGOCDPASS


----------------------------------------------------------------------------------------

# Probar manifiestos de argocd

kubectl apply -f argocd-manifests/repository.yaml
kubectl apply -f argocd-manifests/project.yaml
kubectl apply -f argocd-manifests/application.yaml

kubectl port-forward svc/argocd-demo-hello-world -n argocd-demo 8081:80