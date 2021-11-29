# Logging within kubernetes

This exercise creates a pod on the cluster which logs the timestamp.

With kubectl the logs can be viewed.

## Create log container in default namespace

View counter pod:

```bash
cat counter-pod.yaml
```

Apply:

```bash
kubectl apply -f counter-pod.yaml
```

## Look at the logs of this pod

```bash
kubectl logs counter
```

To stream the log of the gen container in the logme pod (like `tail -f`), do:

```bash
kubectl logs -f --since=10s counter
```

## Clean up

```bash
kubectl delete -f counter-pod.yaml
```
