# Ingress

Ingress is the built‑in Kubernetes load‑balancing framework for HTTP traffic. With Ingress, you control the routing of external traffic. When running on public clouds like AWS or GKE the load-balancing feature is available out of the box.

**Why Ingress?**
For each service with LoadBalancer type, AWS/GCP/Azure will create a new ELB (which comes with costs if you have a lot of services). With Kubernetes ingress you will need only one for one IP address. There are several Ingress controllers like NGINX/NGINX Plus, Traefik, Voyager (HAProxy) or Contour (Envoy) but also Amazon and Google offer Ingress implementations (AWS Aplication Load Balancer or Google Cloud Load Balancer)

Ingress is the most useful if you want to expose multiple services under the same IP address, and these services all use the same L7 protocol (typically HTTP). You can get a lot of features out of the box (like SSL, Auth, Routing, etc) depending on the ingress implementation.

Read more about Load Balancer and Ingress Controllers here:
https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0

We will use the Nginx ingress controller which is probably the most used at the moment.
https://cloud.google.com/community/tutorials/nginx-ingress-gke

## Install

Install  NGINX using Helm:

```bash
helm upgrade -i nginx-ingress stable/nginx-ingress \
--namespace ingress-nginx \
--set controller.stats.enabled=true \
--set controller.metrics.enabled=true \
--set controller.podAnnotations."prometheus\.io/scrape"=true \
--set controller.podAnnotations."prometheus\.io/port"=10254
```

Get IP address:

```
kubectl get service -n ingress-nginx nginx-ingress-controller
```

## Cleanup 

```
helm delete --purge nginx-ingress
```
