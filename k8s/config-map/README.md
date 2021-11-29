# Config Maps

ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable. 

## Define a container environment variable with data from a single ConfigMap

Create config map:

```
kubectl create configmap kubernetes-config --from-literal=kubernetes.training=fundamentals
```

Describe config map:

```
kubectl describe configmaps kubernetes-config
```

Create pod with config map as environment variable:

```
cat pod-env-cm.yaml
kubectl apply -f  pod-env-cm.yaml
```

View result:

```
kubectl describe pod pod-env-cm
kubectl logs pod-env-cm
```

## Add ConfigMap data to a Volume

Create config map:

```bash
kubectl apply -f cm-application.yaml
```

Describe config map:

```bash
kubectl describe configmaps application-config
```

Create pod with config map as volume :

```bash
cat pod-vol-cm.yaml
kubectl apply -f  pod-vol-cm.yaml
```

View result:

```bash
kubectl describe pod pod-vol-cm
kubectl logs pod-vol-cm
```

## Clean up

```bash
kubectl delete -f pod-env-cm.yaml
kubectl delete -f pod-vol-cm.yaml
kubectl delete -f cm-application.yaml
kubectl delete configmap kubernetes-config
```
