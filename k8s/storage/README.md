# Storage

How to configure a Pod to use a Volume for storage.

A Container's file system lives only as long as the Container does. So when a Container terminates and restarts, filesystem changes are lost. For more consistent storage that is independent of the Container, you can use a Volume. This is especially important for stateful applications, such as key-value stores (such as valkey, a redis replacement, after the [redis rugpull](https://en.wikipedia.org/wiki/Redis#History)) and databases.

## Create valkey Pod

In this exercise, you create a Pod that runs one Container. This Pod has a Volume of type emptyDir that lasts for the life of the Pod, even if the Container terminates and restarts. Here is the configuration file for the Pod.

Create the Pod:

```bash
kubectl apply -f valkey.yaml
```

Verify that the Pod's Container is running, and then watch for changes to the Pod:

```bash
kubectl get pod valkey -w
```

In another terminal, get a shell to the running Container:

```bash
kubectl exec -it valkey -- /bin/bash
```

Go to `/data`, and then create a file:

```bash
valkey@valkey:/data# cd /data/
valkey@valkey:/data# echo Hello > test-file
valkey@valkey:/data# cat test-file
Hello
```

Kill the valkey process, the main process in Docker will always have PID 1:

```bash
valkey@valkey:/data# kill 1
```

In your original terminal, watch for changes to the valkey Pod. Eventually, you will see something like this:

```
NAME      READY     STATUS     RESTARTS   AGE
valkey     1/1       Running    0          13s
valkey     0/1       Completed  0         6m
valkey     1/1       Running    1         6m
```

At this point, the Container has terminated and restarted. This is because the valkey Pod has a restartPolicy of Always.

Get a shell into the restarted Container:

```bash
kubectl exec -it valkey -- /bin/bash
```

In your shell, go to /data, and verify that test-file is still there.

```bash
valkey@valkey:/data# ls -l /data/
total 4
-rw-r--r-- 1 valkey valkey 6 Oct  4 10:14 test-file
valkey@valkey:/data$ cat test-file
Hello
valkey@valkey:/data$
```

## Clean-up

```bash
kubectl delete -f valkey.yaml
```
