# Labels

Labels are the mechanism you use to organize Kubernetes objects. A label is a key-value pair, without any pre-defined meaning. So you’re free to choose labels as you see fit, for example, to express environments such as 'this pod is running in development'.

Let’s create a pod that initially has one label (env=development):

```bash
kubectl apply -f pod.yaml
```

Show labels:

```bash
kubectl get pods --show-labels
```

You can add a label to the pod as:
Change <YOUR_NAME> with your name! :-)

```bash
kubectl label pods hello-labels owner=<YOUR_NAME>
```
use with --overwrite to change the value, by default boolean parameters are true for example when you write --overwrite is the same as --overwrite=true

Example to list only pods that have an owner that equals `YOUR_NAME`, use the --selector option:

```bash
kubectl get pods --selector owner=<YOUR_NAME>
```

Clean-up:

```bash
kubectl delete -f pod.yaml
```
