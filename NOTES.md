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

## Per-Route Metrics
To have per-route metrics we have to generate a service profile, we can generate based on OpenAPI spec and protobuf. Here is the command to generate Service Profile based on given OpenAPI spec.
```
linkerd profile -n emojivoto --open-api web.swagger web-svc > web-sp.yml
```
And here's the command to generate Service Profile based on given protobuf file.
```
linkerd profile -n emojivoto --proto Voting.proto voting-svc > voting-sp.yml
```
The next step is apply those service profile YAML files with `kubectl apply -f <FILE_NAME>`

To check the metrics of our service using Linkerd viz extension, run this command.
```
linkerd viz routes service/web-svc --namespace emojivoto
```