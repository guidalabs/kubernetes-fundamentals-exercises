# Blue Green

A blue/green deployment differs from a ramped deployment because the “green” version of the application is deployed alongside the “blue” version. After testing that the new version meets the requirements, we update the Kubernetes Service object that plays the role of load balancer to send traffic to the new version by replacing the version label in the selector field.

![bluegreen](blue-green.gif "Blue-Green")

## Deploy Blue v1

```
kubectl apply -f hello-world-blue.yaml
```

```
kubectl get pods
```

## Test

Get IP address:

```
export IP_ADDRESS=`kubectl -n ingress-nginx get svc nginx-ingress-controller -o json | jq -r '.status.loadBalancer.ingress[0].ip'` 
```

With CURL:

```
curl $IP_ADDRESS/api
```

And in browser you should see the blue version (1.0.0):

<http://$IP_ADDRESS>

## Deploy green v2

```
kubectl apply -f hello-world-green.yaml
```

```
kubectl get pods -w
```

Stop with CONTROL-C


## Test

With CURL:

```
curl $IP_ADDRESS/api
```

And in browser you should see the blue version (1.0.0):

<http://$IP_ADDRESS>

## Switch to green

Check that service is pointing to `blue` version:

```
kubectl describe svc hello-world-service
```

Change to `green` version:

```
kubectl patch service hello-world-service -p '{"spec":{"selector":{"app":"hello-world-green"}}}'
```

## Test

With CURL:

```
curl $IP_ADDRESS/api
```

And in browser you should see the green version (2.0.0):

<http://$IP_ADDRESS>

# Clean up

```
kubectl delete -f hello-world-blue.yaml
kubectl delete -f hello-world-green.yaml
```
