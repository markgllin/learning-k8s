# K8s - Container Orchestration Tool
## References
- [K8s API reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.31/#)
- [kubectl Quick reference](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
- [kubectl (full) reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
- [helm cli reference](https://helm.sh/docs/helm/)
## Common General Commands
#### `kubectl get all`:
- returns all components created in the cluster
- does not include configmap/secret

#### `kubectl get configmap`:
- returns config maps

#### `kubectl get secret`:
- returns secrets

#### `kubectl describe <component> <component-name>`:
- returns additional details on k8s resource

Examples:
```
kubectl describe service webapp-service
kubectl describe deployment webapp-deployment
kubectl describe pod mongo-deployment-77b4cbd6b-kmgw7
```

#### `kubectl logs <podName>`:
- view logs of container/pod

Examples:
```
kubectl logs webapp-deployment-79656559cb-j7rgd

# to stream logs as the come in
kubectl logs -f webapp-deployment-79656559cb-j7rgd
```

#### `kubectl get node`:
- get name of node

Examples:
```
kubectl get node

# wide output
# will output ip address of the node as well
kubectl get node -o wide
```
## Cleaning up
```
kubectl delete <resource-type> <resource-name>

# e.g:
kubectl delete deployment webapp-deployment
kubectl delete service mongo-deployment
```

# Helm
Multiple features:
- package manager
- templating engine
- release management

## Package Manager
Package Manager for K8s. Helm charts are a bundle of pre-written *.yaml files for provisioning common deployments (e.g. ELK stack, db apps etc.).


## Templating Engine
If deploying multiple microservices where resources are mostly the same (e.g. differs only in image, labels, etc), can create a template file to reuse resource. Input parameters can then be supplied through a `values.yaml` file (or `--set` flag).

Example:
```
# template.yaml
apiVersion: v1
kind: Pod
metadata:
  name: {{ .Values.name }}
spec:
  containers:
  - name: {{ .Values.container.name }}
    image: {{ .Values.container.image }}
    port: {{ .Values.container.port }}

# values.yaml
name: my-app
container:
  name: my-app-container
  image: my-app-image
  port: 9001
```
## Release Management
Key differences b/w Helm v2 & v3.

Helm v2 comes in 2 parts:
- Helm Client (i.e. helm CLI)
  - when executing command to install chart, command is sent to Tiller to execute request
- Helm Server (i.e. Tiller)
  - must operate within k8s cluster
  - stores a copy of helm chart configurations that client sends.
  - Tracks updates, changes, and executions of chart
  - changes applied to existing deployment instead of creating new one
  - can handle rollbacks
  - lots of power/security concerns as it can create, update, delete resources inside k8s cluster

Helm v3:
- removes Tiller component to address security concerns
- as a result, no longer has release management feature

## Helm Chart Structure
```
mychart/          # name of chart
  chart.yaml      # meta info about chart (e.g. name, version, dependencies etc.)
  values.yaml     # values for the template files (contains default values that can be overridden)
  charts/         # chart dependencies (i.e. other charts)
  templates/      # where template files are stored
  ...
```

## Common General Commands
### `helm repo add <repo-name> <repo-source>`
- adds a helm repo as a source

### `helm search repo <repo-name>`
- show all charts available in repo

### `helm install <what-to-name-deployment> <chartname>`
- template files will be filled with values from `values.yaml`
- deploys resources in chart
- can override defaults

Examples
``` 
helm install mongodb --values test-mongodb.yaml bitnami/mongodb
or
helm install --set version=2.0.0 
or
helm install --values=my-values.yaml <chartname>

# default values.yaml
imageName: myapp
port: 8080
version: 1.0.0

# override with my-values.yaml
version: 2.0.0

# result
imageName: myapp
port: 8080
version: 2.0.0
```
### `helm upgrade <chartname>`
- upgrades chart with new revision

### `helm rollback <chartname>`
- rollback to prior version of chart

### `helm uninstall <deployment-name>`
- removes all resources associated with deployment