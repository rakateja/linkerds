# Brain-dump notes
## Deploy Linkerd to Kubernetes cluster

Install Linkerd CRDs
```
linkerd install --crds | kubectl 
```

Install Linkerd control plane
```
linkerd install | kubectl apply -f -
```
Basically the command above is applying the output of `linkerd install` using `kubectl apply`. This will deploy Linkerd control plane to Kubernetes cluster.

To validate the result of Linkerd deployment to Kubernetes cluster, we can execute `linkerd check`.

## Viz extension
To help us to have metrics and visibility of Linkerd on Kubernetes cluster, we can add viz extension with following command.
```
linkerd viz install | kubectl apply -f -
```

Run this command to check the viz extension status
```
linkerd viz check
```

## Add Linkerd to our services in Kubernetes
Injecting our service deployments with Linkerd proxy by annotating its namespace.
```
kubectl annotate ns emojivoto linkerd.io/inject=enabled
```
Restart the deployments in emojivoto namespace
```
kubectl rollout restart deploy -n emojivoto
```
List pods in emojivoto namespace to check the Linkerd proxy. We'll see that the number of container in each pod is increased from 1 to 2.
```
kubectl get pods -n emojivoto
```