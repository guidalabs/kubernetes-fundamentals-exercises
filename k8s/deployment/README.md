# Rolling Update

A ramped deployment updates pods in a rolling update fashion, a secondary ReplicaSet is created with the new version of the application, then the number of replicas of the old version is decreased and the new version is increased until the correct number of replicas is reached.

![rolling-update](rollingupdate.gif "Rolling Update")


## Deploy App

Create app:

```bash
kubectl apply -f hello-world.yaml
```

```bash
kubectl rollout status deployment hello-world -w
```

Get events:

```bash
kubectl get events
```

See pod is running with IPs:

```bash
kubectl get pods -o wide
```

## Create Service (ClusterIP, NodePort, LoadBalancer)

```bash
kubectl apply -f hello-world-service.yaml
```

```bash
kubectl get service
```

## Create Ingress

Ingress is the built‑in Kubernetes load‑balancing framework for HTTP traffic. With Ingress, you control the routing of external traffic. Ingress is the most useful if you want to expose multiple services under the same IP address, and these services all use the same L7 protocol (typically HTTP). You can get a lot of features out of the box (like SSL, Auth, Routing, etc) depending on the ingress implementation.

Update `host` field in `hello-world-ingress.yaml` with `hello-yourname.guida-poc1.app.guida.io`

```bash
kubectl apply -f hello-world-ingress.yaml
```

## Test

With CURL:

```bash
curl hello-yourname.guida-poc1.app.guida.io/api
```

Or in browser:

<http://hello-yourname.guida-poc1.app.guida.io>

You should see the pod name and version of the app.

# Self healing

Stop pod:

```bash
kubectl delete pod hello-world
```

See what happened and if a new pod is started:

```bash
kubectl get events
```

```bash
kubectl get pods
```

# Healtcheck

Check health check:

```bash
curl hello-yourname.guida-poc1.app.guida.io/health
```

Marks server as down:

```bash
curl -X POST hello-yourname.guida-poc1.app.guida.io/down
```

Check health check:

```bash
curl hello-yourname.guida-poc1.app.guida.io/health
```

Check that health check is failed:

```bash
kubectl get events -w
```

```bash
kubectl get pods -o wide
```

# Update & Rollback

Update App to use new version

```bash
kubectl set image --record deployments/hello-world hello-world=avthart/hello-app:1.0.1
```

Roll out status

```bash
kubectl rollout status deployments/hello-world
```

```bash
kubectl get pods
```

```bash
kubectl describe pod hello-world
```

Which version is running? Stil alive?

```bash
curl hello-yourname.guida-poc1.app.guida.io/api
```

Rollback App

```bash
kubectl rollout history deployments/hello-world
```

```bash
kubectl rollout history deployments/hello-world --revision=2
```

```bash
kubectl rollout undo deployments/hello-world
```

Update to working version:

```bash
kubectl set image --record deployments/hello-world hello-world=avthart/hello-app:1.0.3
```

```bash
kubectl rollout status deployments/hello-world
```

Which version is running? Was there any downtime?

```bash
curl hello-yourname.guida-poc1.app.guida.io/api
```

# Scaling

Scale app to 3 instances

```bash
kubectl scale --replicas=3 deployment/hello-world
```

```bash
kubectl rollout status deployments/hello-world
```

```bash
kubectl get pods
```

Check that request is handled by all instances:

```bash
while sleep 0.5; do curl hello-yourname.guida-poc1.app.guida.io/api; done
```

Stop with CONTROL-C

# Clean up

```bash
kubectl delete -f hello-world-ingress.yaml
kubectl delete -f hello-world-service.yaml
kubectl delete -f hello-world.yaml
```
