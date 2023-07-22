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