# Network Policies

NetworkPolicies can be configured to control Pod traffic flows at the IP address or port level. The entities that a Pod can communicate with are identified through a combination of the following 3 identifiers:

- Other pods that are allowed (exception: a pod cannot block access to itself)
- Namespaces that are allowed
- IP blocks (exception: traffic to and from the node where a Pod is running is always allowed, regardless of the IP address of the Pod or the node)
- FQDN's (CiliumNetworkpolicy only)

Read more about it at: (https://kubernetes.io/docs/concepts/services-networking/network-policies/)
If you run Cilium as container network interface you can use CiliumNetworkpolicies. Read more about it at: https://docs.cilium.io/en/stable/network/kubernetes/policy/

In this exercise we are going to look at Pod to Pod communication within a namespace. First we are deploying a Frontend and an API pod in your namespace. The pods will both have a service and should NOT be allowed to communicate with eachother because there is a default deny networkpolicy:

```bash
kubectl apply -f frontend.yaml
kubectl apply -f frontend-svc.yaml
kubectl apply -f api.yaml
kubectl apply -f api-svc.yaml
kubectl get pod,svc
```

Validate that the pods are NOT able to communicate.

```bash
kubectl exec -it frontend -- curl api:9898
kubectl exec -it api -- curl frontend:9898
```

Deploy a network policy that will allow egress traffic from the Frontend pod to the API pod.
```bash
kubectl apply -f allow-egress-frontend-to-api.yaml
```

Connectivity is still not working because there is only a networkpolicy deployed that allows egress traffic from the Frontend pod. Figure out a way to deploy a Cilium networkpolicy that allows Ingress traffic to the API pod from the Frontend pod. Use the Cilium network policy editor (https://editor.networkpolicy.io/) and the example in allow-ingress-api-from-frontend.yaml. Once you deployed the networkpolicy, validate that you can connect from the frontend to the API pod again.

Bonus: find a partner that is also working on this exercise and create networkpolicies that will allow connectivity from your Frontend pod to your partners API pod. Tip: combine namespaceSelectors and podSelectors like the example below:

```
    - toEndpoints:
      - matchLabels:
          io.kubernetes.pod.namespace: neighbour
          app: neighbour
```

When you are validating the connectivity keep in mind that a service in another namespace can be reached as follows: \<SERVICE_NAME\>.\<NAMESPACE\>

Cleanup resources
```
kubectl delete pods --all
kubectl delete svc --all
kubectl delete ciliumnetworkpolicy --all
```
