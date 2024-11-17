### `kubectl get all`:
- returns all components created in the cluster
- does not include configmap/secret

### `kubectl get configmap`:
- returns config maps

### `kubectl get secret`:
- returns secrets

### `kubectl describe <component> <component-name>`:
- returns additional details on k8s resource

Examples:
```
kubectl describe service webapp-service
kubectl describe deployment webapp-deployment
kubectl describe pod mongo-deployment-77b4cbd6b-kmgw7
```

### `kubectl logs <podName>`:
- view logs of container/pod

Examples:
```
kubectl logs webapp-deployment-79656559cb-j7rgd

# to stream logs as the come in
kubectl logs -f webapp-deployment-79656559cb-j7rgd
```

### `kubectl get node`:
- get name of node

Examples:
```
kubectl get node

# wide output
# will output ip address of the node as well
kubectl get node -o wide

###