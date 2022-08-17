# Deploy app via Helm

Add helm repository:

```bash
helm repo add podinfo https://stefanprodan.github.io/podinfo
```

Update `podinfo.yaml` with your name and domain (replace \<YOUR_NAME\> on line 4 & 9).

Install pod info app:

```bash
helm upgrade -i podinfo podinfo/podinfo -f podinfo.yaml
```

See pod info resources:

```bash
kubectl get ing,all
```

See that the pod is working and available under `<YOUR_NAME>.<YOUR_DOMAIN>` in your browser.

Update replicas to 2 in `podinfo.yaml` and see that different instances are serving HTTP requests.

Clean up:

```bash
helm delete podinfo
```
