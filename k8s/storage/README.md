# Storage

How to configure a Pod to use a Volume for storage.

A Container's file system lives only as long as the Container does. So when a Container terminates and restarts, filesystem changes are lost. For more consistent storage that is independent of the Container, you can use a Volume. This is especially important for stateful applications, such as key-value stores (such as Redis) and databases.

## Create Redis Pod

In this exercise, you create a Pod that runs one Container. This Pod has a Volume of type emptyDir that lasts for the life of the Pod, even if the Container terminates and restarts. Here is the configuration file for the Pod.

Create the Pod:

```bash
kubectl apply -f redis.yaml
```

Verify that the Pod's Container is running, and then watch for changes to the Pod:

```bash
kubectl get pod redis -w
```

In another terminal, get a shell to the running Container:

```bash
kubectl exec -it redis -- /bin/bash
```

Go to `/data/redis`, and then create a file:

```bash
redis@redis:/data# cd /data/redis/
redis@redis:/data/redis# echo Hello > test-file
redis@redis:/data/redis# cat test-file
Hello
```

Kill the Redis process, the main process in Docker will always have PID 1:

```bash
redis@redis:/data/redis# kill 1
```

In your original terminal, watch for changes to the Redis Pod. Eventually, you will see something like this:

```
NAME      READY     STATUS     RESTARTS   AGE
redis     1/1       Running    0          13s
redis     0/1       Completed  0         6m
redis     1/1       Running    1         6m
```

At this point, the Container has terminated and restarted. This is because the Redis Pod has a restartPolicy of Always.

Get a shell into the restarted Container:

```bash
kubectl exec -it redis -- /bin/bash
```

In your shell, go to /data/redis, and verify that test-file is still there.

```bash
redis@redis:/data/redis# ls -l /data/redis/
total 4
-rw-r--r-- 1 redis redis 6 Oct  4 10:14 test-file
```

## Clean-up

```bash
kubectl delete -f redis.yaml
```