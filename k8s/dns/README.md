# DNS

Service discovery is the process of figuring out how to connect to a service. You can use DNS-based service discovery in your Kubernetes Cluster.

Let’s create a service named `hello-service` and a pod `hello`:

```bash
kubectl apply -f pod.yaml
kubectl apply -f svc.yaml
kubectl get pod,svc
```

Try to use the Kubernetes DNS to request /api on the `hello` pod using service `hello-service`.
```
kubectl apply -f dnsutils.yaml
kubectl exec -i -t dnsutils -- wget -qO- http://hello-service:8080/api
```

To access a service that is deployed in a different namespace than the one you’re accessing it from, use a FQDN in the form `$SVC.$NAMESPACE.svc.cluster.local`.

Or more advanced troubleshooting with dnsutils

We use a modified version with no default namespace in it!
```
kubectl exec -i -t dnsutils -- nslookup kubernetes.default
kubectl exec -i -t dnsutils -- nslookup <YOUR_SERVICE_NAME>.<YOUR_NAME_SPACE>.svc.cluster.local
```

source: https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/

Clean up:

```bash
kubectl delete -f pod.yaml
kubectl delete -f svc.yaml
kubectl delete -f dnsutils.yaml
```
