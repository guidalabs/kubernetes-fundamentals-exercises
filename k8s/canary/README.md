# Canary

A canary deployment consists of routing a subset of users to a new functionality. In Kubernetes, a canary deployment can be done using two Deployments with common pod labels. One replica of the new version is released alongside the old version. Then after some time and if no error is detected, scale up the number of replicas of the new version and delete the old deployment.

![canary](canary.gif "Canary")

## Deploy v1

```
kubectl  apply -f hello-world-v1.yaml
```

## Test

Get IP address:

```bash
export IP_ADDRESS=`kubectl -n ingress-nginx get svc nginx-ingress-controller -o json | jq -r '.status.loadBalancer.ingress[0].ip'`
```

Test:

```bash
curl http://$IP_ADDRESS/api
```

Or in browser:
<http://$IP_ADDRESS>


## Deploy v2 besides v1

```
kubectl  apply -f hello-world-v2.yaml
```

Check that requests are handled by v1 and v2

```
while sleep 0.5; do curl $IP_ADDRESS/api; done
```
```powershell
while (Start-Sleep -Milliseconds 500) { curl hello-yourname.<YOUR_DOMAIN>/api }
```

Stop with CONTROL-C

# Scale v2 to 3

```
kubectl scale --replicas=3 deployment/hello-world-v2
```

```
kubectl  get pods -w
```

Test:

```bash
curl http://$IP_ADDRESS/api
```

Or in browser:
<http://$IP_ADDRESS>

# Delete v1

```
kubectl delete deployment/hello-world-v1
```

```
kubectl get pods
```

Test:

```bash
curl http://$IP_ADDRESS/api
```

Or in browser:
<http://$IP_ADDRESS>

# Clean up

```
kubectl delete -f hello-world-v1.yaml
kubectl delete -f hello-world-v2.yaml
```
