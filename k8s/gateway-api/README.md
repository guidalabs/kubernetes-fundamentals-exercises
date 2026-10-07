# Gateway API

The Gateway API splits traffic management into roles: the platform team runs a shared `Gateway`, and application teams attach their own listeners and routes to it. A `ListenerSet` lets you add a listener (port + hostname) to a shared Gateway from your own namespace, so participants don't clash with each other. `HTTPRoute`s then define how requests on that listener are handled. Implementation-specific features, like timeouts and rate limiting in Envoy Gateway, are configured with policies such as `BackendTrafficPolicy`.

Read more about it at: https://gateway-api.sigs.k8s.io/ and https://gateway.envoyproxy.io/

## Deploy App

```bash
kubectl apply -f hello-world.yaml
kubectl apply -f network-policy.yaml
kubectl get pods,svc
```

## Create ListenerSet

Update `hostname` in `listenerset.yaml` with `<YOUR_NAME>.<YOUR_DOMAIN>`.

```bash
kubectl apply -f listenerset.yaml
kubectl get listenerset hello-world -o yaml
```

Check the `status` to see if the listener is accepted by the shared Gateway.

## Create HTTPRoutes

The first route sends all traffic to the `hello-world` service and adds security headers to the response, like HSTS and a Content Security Policy. The second route redirects requests on `/secure` to HTTPS with a `301`. The most specific path match wins, so `/secure` is handled by the redirect route.

```bash
kubectl apply -f httproute.yaml
kubectl apply -f httproute-redirect.yaml
kubectl get httproute
```

Check the response headers:

```bash
curl -i http://<YOUR_NAME>.<YOUR_DOMAIN>/api
```

Note: browsers ignore HSTS over plain HTTP, it only has effect when served over HTTPS.

Check the redirect, look at the `Location` header (there is no HTTPS listener, so following the redirect will fail):

```bash
curl -i http://<YOUR_NAME>.<YOUR_DOMAIN>/secure
```

## Create BackendTrafficPolicy

The policy sets connect and request timeouts, and limits every unique client IP to 10 requests per minute.

```bash
kubectl apply -f backendtrafficpolicy.yaml
kubectl get backendtrafficpolicy hello-world -o yaml
```

Send some requests and see the `429 Too Many Requests` once the limit is reached:

```bash
for i in $(seq 1 15); do curl -s -o /dev/null -w "%{http_code}\n" http://<YOUR_NAME>.<YOUR_DOMAIN>/api; done
```

## Clean up

```bash
kubectl delete -f backendtrafficpolicy.yaml
kubectl delete -f httproute-redirect.yaml
kubectl delete -f httproute.yaml
kubectl delete -f listenerset.yaml
kubectl delete -f network-policy.yaml
kubectl delete -f hello-world.yaml
```
