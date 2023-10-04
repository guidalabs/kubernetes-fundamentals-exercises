# Kubectl

The [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) command line tool lets you control Kubernetes clusters. For configuration, kubectl looks for a file named config in the `$HOME/.kube` directory. You can specify other kubeconfig files by setting the `KUBECONFIG` environment variable or by setting the `--kubeconfig` flag.

## Install

[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Test (can be done later when configured the .kube fike)

Verify that kubectl is working:

```bash
kubectl version
```
