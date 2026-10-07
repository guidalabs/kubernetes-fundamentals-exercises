# Deploy app via Helm

Update `podinfo.yaml` with your name and domain (replace \<YOUR_NAME\> on line 5 & 29).

Install pod info app:

```bash
helm upgrade --install podinfo --values podinfo.yaml oci://ghcr.io/stefanprodan/charts/podinfo
kubectl apply -f network-policy.yaml
```

See pod info resources:

```bash
kubectl get httproute,all
```

See that the pod is working and available under `<YOUR_NAME>.<YOUR_DOMAIN>` in your browser.

Update replicas to 2 in `podinfo.yaml` and see that different instances are serving HTTP requests.

Clean up:

```bash
helm delete podinfo
kubectl delete -f network-policy.yaml
```
