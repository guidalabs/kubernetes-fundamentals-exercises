# A/B Testing

A/B testing is really a technique for making business decisions based on statistics, rather than a deployment strategy. However, it is related and can be implemented using a canary deployment so we will briefly discuss it here.In addition to distributing traffic amongst versions based on weight, you can precisely target a given pool of users based on a few parameters (cookie, user agent, etc.). This technique is widely used to test conversion of a given feature and only rollout the version that converts the most.

![ab](a-b.gif "A/B Testing")

> It is important that the canary ingress has the exact same name as the production ingress to ensure that the controller puts the configuration in the right section of nginx configuration. That means that you will need a second namespace as it is not possible to create a second resource with the same name in a single namespace

Also see https://medium.com/@domi.stoehr/canary-deployments-on-kubernetes-without-service-mesh-425b7e4cc862

## Create namespace

```
kubectl create namespace ab-v1
kubectl create namespace ab-v2
```

## Deploy v1

```
kubectl -n ab-v1 apply -f hello-world-v1.yaml
```

## Create Ingress

```
kubectl -n ab-v1 apply -f hello-world-ingress.yaml
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

HTTP requests should be routed to v1

## Deploy v2 besides v1 in another namespace

```
kubectl -n ab-v2 apply -f hello-world-v2.yaml
```

## Route requests by header to v2

```
kubectl -n ab-v2 apply -f hello-world-ingress-v2.yaml
```

## Test

Check that requests are handled by v2

```
curl -H "Canary: always" $IP_ADDRESS/api
```

## Clean up

```
kubectl delete ns ab-v1 ab-v2
```