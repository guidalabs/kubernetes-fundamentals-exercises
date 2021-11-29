# Debugging Applications

Deploy:

```bash
kubectl apply -f troubleshooting.yaml
```

Find out why deployment `hello-world` is not started and healthy.

Commands that you can use:

```
kubectl logs ${POD_NAME} -c ${CONTAINER_NAME}
kubectl describe pod ${POD_NAME}
kubectl describe deployment ${DEPLOYMENT_NAME}
kubectl get events
```

If you have found the errors, change the manifests and rerun kubectl apply so that the application will be working.

> Hint: there are two configuration mistakes...

Clean up:

```bash
kubectl delete -f troubleshooting.yaml
```
