# Kubernetes Fundamentals Exercises

This project contains labs for the Kubernetes Fundamentals training.

In this workshop we will use the application:
* <https://github.com/avthart/hello-app>

## Install software

* [Kubectl](k8s/kubectl/)
* [Helm](k8s/helm/)

## Other pre-requisites (provided by your trainer)

- Network details
- A personal namespace
- An instruction to download your kubeconfig file

## Configure kubeconfig
By default, kubectl looks for a file named config in the $HOME/.kube directory. You can specify other kubeconfig files by setting the KUBECONFIG environment variable or by setting the --kubeconfig flag. Make your life easier with the kubectl cheatsheet (https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

1. Copy the content of your kubeconfig file and write to ~/.kube/config.
2. Test your connection with the following commands:

```
kubectl version
```
Note, backend and client version, backend version implies you have a connection.
```
kubectl get pods -n <NAMESPACE>
```
3. Permanently save the namespace for all subsequent kubectl commands with the following command:
```
kubectl config set-context --current --namespace=<NAMESPACE>
```

## Start point exercises

Now you are ready to do some κυβερνήτες !

Next you will find the actual exercises with links, for the terminal find them under the folder ./k8s/<xyz>/

## Deployment, Service, Ingress

* [Deployment](k8s/deployment/)

## Configuration

* [Configmap](k8s/configmap/)

* [Secrets](k8s/secrets/)

## Labels

* [Labels](k8s/labels/)

## Storage

* [Storage](k8s/storage/)

## Networking

* [DNS](k8s/dns/)
* [Network Policies](k8s/network-policies/)

## Troubleshooting

* [Troubleshooting](k8s/debugging/)

## Cronjob

* [Cronjob](k8s/cronjob/)

## Horizontal Pod Autoscaler

* [Horizontal Pod Autoscaler](k8s/hpa/)

## Helm

* [Deploy app using Helm](k8s/helm-app/)

## Extra exercises

* [Install Wordpress using Helm](https://github.com/bitnami/charts/tree/master/bitnami/wordpress) - Hint: Enable ingress using vars `ingress.*` and set `service.type` to `ClusterIp`
* Create an app deployment (e.g. nginx) from scratch  with a liveness probe, replicas, ingress, resource limits and configmap

## Extra Resources

* [Helm](https://helm.sh/docs/intro/quickstart)
* [Ingress Routing](https://kubernetes.io/docs/concepts/services-networking/ingress/#what-is-ingress)
* [Docker Compose for Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/translate-compose-kubernetes/)
* [Killercoda Labs](https://killercoda.com/killer-shell-ckad)

## Unwind

```
unset KUBECONFIG
```
and / or revert to your old config backed-up earlier

```
cp ~/.kube/configBackup ~/.kube/config
```
