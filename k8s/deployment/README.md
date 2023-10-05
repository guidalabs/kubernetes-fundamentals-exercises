# Rolling Update

A ramped deployment updates pods in a rolling update fashion, a secondary ReplicaSet is created with the new version of the application, then the number of replicas of the old version is decreased and the new version is increased until the correct number of replicas is reached.

![rolling-update](rollingupdate.gif "Rolling Update")


## Deploy App

Create app:

```bash
kubectl apply -f hello-world.yaml
```

As you can see this app isn't allowed on the cluster by our policy engine Kyverno.
As seen in the message certain `securityContext` settings are required:
```
Error from server: error when creating "../deployment/hello-world.yaml": admission webhook "validate.kyverno.svc-fail" denied the request:

resource Deployment/playground/hello-world was blocked due to the following policies

disallow-capabilities-strict:
  autogen-require-drop-all: 'validation failure: Containers must drop `ALL` capabilities.'
disallow-privilege-escalation:
  autogen-privilege-escalation: 'validation error: Privilege escalation is disallowed.
    The fields spec.containers[*].securityContext.allowPrivilegeEscalation, spec.initContainers[*].securityContext.allowPrivilegeEscalation,
    and spec.ephemeralContainers[*].securityContext.allowPrivilegeEscalation must
    be set to `false`. rule autogen-privilege-escalation failed at path /spec/template/spec/containers/0/securityContext/'
require-run-as-nonroot:
  autogen-run-as-non-root: 'validation error: Running as root is not allowed. Either
    the field spec.securityContext.runAsNonRoot must be set to `true`, or the fields
    spec.containers[*].securityContext.runAsNonRoot, spec.initContainers[*].securityContext.runAsNonRoot,
    and spec.ephemeralContainers[*].securityContext.runAsNonRoot must be set to `true`.
    rule autogen-run-as-non-root[0] failed at path /spec/template/spec/securityContext/runAsNonRoot/
    rule autogen-run-as-non-root[1] failed at path /spec/template/spec/containers/0/securityContext/'
restrict-seccomp-strict:
  autogen-check-seccomp-strict: 'validation error: Use of custom Seccomp profiles
    is disallowed. The fields spec.securityContext.seccompProfile.type, spec.containers[*].securityContext.seccompProfile.type,
    spec.initContainers[*].securityContext.seccompProfile.type, and spec.ephemeralContainers[*].securityContext.seccompProfile.type
    must be set to `RuntimeDefault` or `Localhost`. rule autogen-check-seccomp-strict[0]
    failed at path /spec/template/spec/securityContext/seccompProfile/ rule autogen-check-seccomp-strict[1]
    failed at path /spec/template/spec/containers/0/securityContext/'
```

So lets try again with a secured deployment (see the added `securityContext` in `hello-world-secure.yaml`):

```bash
kubectl apply -f hello-world-secure.yaml
```

Watch it being created:

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

Update `host` field in `hello-world-ingress.yaml` with `hello-yourname.<YOUR_DOMAIN>`

```bash
kubectl apply -f hello-world-ingress.yaml
```

## Test

With CURL:

```bash
curl hello-yourname.<YOUR_DOMAIN>/api
```

Or in browser:

<http://hello-yourname.<YOUR_DOMAIN>>

You should see the pod name and version of the app.

# Self healing

Stop pod:

```bash
kubectl delete pod -l app=hello-world
```

See what happened and if a new pod is started:

```bash
kubectl get events
```

```bash
kubectl get pods
```

# Healthcheck

Check health check:

```bash
curl hello-yourname.<YOUR_DOMAIN>/health
```

Marks server as down:

```bash
curl -X POST hello-yourname.<YOUR_DOMAIN>/down
```
```powershell
Invoke-WebRequest -Uri http://hello-yourname.<YOUR_DOMAIN>/down -Method POST
```

Check health check:

```bash
curl hello-yourname.<YOUR_DOMAIN>/health
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
kubectl set image deployments/hello-world hello-world=avthart/hello-app:1.0.1
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
curl hello-yourname.<YOUR_DOMAIN>/api
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
kubectl set image deployments/hello-world hello-world=avthart/hello-app:1.0.3
```

```bash
kubectl rollout status deployments/hello-world
```

Which version is running? Was there any downtime?

```bash
curl hello-yourname.<YOUR_DOMAIN>/api
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
while sleep 0.5; do curl hello-yourname.<YOUR_DOMAIN>/api; done
```
```powershell
while (Start-Sleep -Milliseconds 500) { curl hello-yourname.<YOUR_DOMAIN>/api }
```

Stop with CONTROL-C

# Clean up

```bash
kubectl delete -f hello-world-ingress.yaml
kubectl delete -f hello-world-service.yaml
kubectl delete -f hello-world.yaml
```
