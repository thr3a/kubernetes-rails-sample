```
docker compose build
```


```
argocd app create rails-sample --repo https://github.com/thr3a/kubernetes-rails-sample-manifests.git \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace rails-sample \
  --path './'
```
