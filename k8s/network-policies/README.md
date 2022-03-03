# Network Policies

NetworkPolicies can be configured to control Pod traffic flows at the IP address or port level. The entities that a Pod can communicate with are identified through a combination of the following 3 identifiers:

- Other pods that are allowed (exception: a pod cannot block access to itself)
- Namespaces that are allowed
- IP blocks (exception: traffic to and from the node where a Pod is running is always allowed, regardless of the IP address of the Pod or the node)

Read more about it at: (https://kubernetes.io/docs/concepts/services-networking/network-policies/)

In this exercise we are going to look at Pod to Pod communication within a namespace. First we are deploying a Frontend and an API pod in your namespace. The pods will both have a service and should be allowed to communicate with eachother:

```bash
kubectl apply -f frontend.yaml
kubectl apply -f frontend-svc.yaml
kubectl apply -f api.yaml
kubectl apply -f api-svc.yaml
kubectl get pod,svc
```

By default, Kubernetes doesn't implement any networkpolicies so validate that the pods are able to communicate.

```bash
kubectl exec -it frontend -- curl api
kubectl exec -it api -- curl frontend
```

Now implement a default deny ingress and egress policy.

```bash
kubectl apply -f default-deny-np.yaml
kubectl get netpol
```

Validate if the networkpolicies are working by trying to connect to the Frontend pod to the Api pod.

```bash
kubectl exec -it frontend -- curl api
```

Deploy a network policy that will allow egress traffic from the Frontend pod to the API pod.
```bash
kubectl apply -f allow-egress-frontend-to-api.yaml
```

Connectivity is still not working because there is only a networkpolicy deployed that allows egress traffic from the Frontend pod. Figure out a way to deploy a networkpolicy that allows Ingress traffic to the API pod from the Frontend pod. Use the Kubernetes documentation (https://kubernetes.io/docs/concepts/services-networking/network-policies/) and the example in allow-ingress-api-from-frontend.yaml. Once you deployed the networkpolicy, validate that you can connect from the frontend to the API pod again.

Bonus: find a partner that is also working on this exercise and create networkpolicies that will allow connectivity from your Frontend pod to your partners API pod. Tip: combine namespaceSelectors and podSelectors like the example below:

```
  egress:
    - to:
      - namespaceSelector:
          matchLabels:
            guida.io/namespace: <NAMESPACE>
        podSelector:
          matchLabels:
            app: <APP_LABEL>
```

When you are validating the connectivity keep in mind that a service in another namespace can be reached as follows: \<SERVICE_NAME\>.\<NAMESPACE\>