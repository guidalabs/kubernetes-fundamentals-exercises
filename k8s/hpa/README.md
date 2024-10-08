# Horizontal Pod Autoscaler

A HorizontalPodAutoscaler automatically updates a workload resource, with the aim of automatically scaling the workload to match the resource demand. Horizontal scaling means that the response to increased load is to deploy more Pods instead of giving the Pod more CPU or Memory. You can configure your own thresholds for when the pod should scale.

Deploy a frontend deployment with a service that will be used for autoscaling.

```
kubectl apply -f frontend.yaml
```

Confirm that the application is running
```
kubectl get pods,svc
```

Now that the server is running, you will create an autoscaler that will scale between 1 and 5 replicas of the Pod depending on the CPU Utilisation. The Deployment will scale when the CPU utilization of the Pod hits 50%.

```
kubectl apply -f hpa.yaml
```

Confirm that the HPA is deployed
```
kubectl get hpa
```

Now increase the load on the frontend by querying the service from another container. Do this from another terminal session.

```
kubectl apply -f loadgenerator.yaml
kubectl exec -it loadgenerator -- /bin/sh -c "while sleep 0.01; do wget -qO- http://frontend:9898; done"
```

From a new terminal session monitor the autoscaling status.

```
kubectl get hpa frontend --watch
```

Within a moment the CPU target is reached and the application will start scaling. Confirm this by checking the pods.

```
kubectl get pods
```

Stop generating load by typing `ctrl+c` in the Pod that runs Busybox.

Then validate that the load is decreasing and the deployment will start scaling down. This might take a while because the default stabilizationWindow is 300 seconds.

```
kubectl get hpa frontend --watch
```

Cleanup your resources:
```
kubectl delete -f frontend.yaml
kubectl delete -f loadgenerator.yaml
kubectl delete -f hpa.yaml
```
