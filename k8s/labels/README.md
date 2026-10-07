# Labels

Labels are the mechanism you use to organize Kubernetes objects. A label is a key-value pair, without any pre-defined meaning. So you’re free to choose labels as you see fit, for example, to express environments such as 'this pod is running in development'.

## Show and Display Labels

Let’s create a pod that initially has one label (env=development):

```bash
kubectl apply -f pod.yaml
```

Show all labels:

```bash
kubectl get pods --show-labels
```

Show specific labels:

```bash
kubectl get pods -L env
```

You can add a label to the pod as:
Change <YOUR_NAME> with your name! :-)

```bash
kubectl label pods hello-labels owner=<YOUR_NAME>
```

```bash
kubectl get pods -L env -L owner
```

Remove the label with `-`:

```bash
kubectl label pods hello-labels owner-
```

> [!NOTE]
> - To set labels instead of adding a label, use `--overwrite` to overwrite the entire list of labels.
> - By default boolean parameters are true, for example `--overwrite` is the same as `--overwrite=true`.

## Filtering on labels

Example to list only pods that have an owner that equals `YOUR_NAME`, use the `-l` option (short for `--selector` option):

```bash
kubectl get pods -l owner=<YOUR_NAME>
```

Put back the owner label, and check again:

```bash
kubectl label pods hello-labels owner=<YOUR_NAME>
```

Clean-up:

```bash
kubectl delete -f pod.yaml
```
