Setup kind
Make sure to use a recent version of kind (0.17 or newer)!
Will not work with older cluster
```
kind create cluster --config=kind.yaml
```

Setup cluster
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.9.2/cert-manager.yaml
kubectl apply -n cert-manager -f cert-issuer.yaml
```

Setup Argo
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Add the following to /etc/hosts
```
127.0.0.1       localhost argocd.local
```

Setup ingress
```
kubectl apply -f ingress.yaml
```

Setup Skiperator
```
kubectl create namespace istio-system
kubectl apply --filename https://raw.githubusercontent.com/istio/istio/1.15.1/manifests/charts/base/crds/crd-all.gen.yaml
kubectl create namespace skiperator-system
kubectl create -f deployment
kubectl create configmap gcp-identity-config --from-literal="workloadIdentityPool=testPool" --from-literal="identityProvider=testProvider" -n skiperator-system
kubectl create configmap instana-networkpolicy-config --from-literal=cidrBlock=192.168.1.0/26 -n skiperator-system
```
