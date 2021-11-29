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
Note, backend and client versionn, backend version implies you have a connection.
```
kubectl get pods -n <NAMESPACE>
```
3. Permanently save the namespace for all subsequent kubectl commands with the following command:
```
kubectl config set-context --current --namespace=<NAMESPACE>
```

## Start point Excercises

Now you are ready to do some κυβερνήτες !

Next you will find the actual exercises with links, for the terminal find them under the folder ./k8s/<xyz>/

## Deployment, Service, Ingress

* [Deployment](k8s/deployment/)

## Configuration

* [Configmap](k8s/config-map/)

* [Secrets](k8s/secrets/)

## Labels

* [Labels](k8s/labels/)

## Storage

* [Storage](k8s/storage/)

## Networking

* [DNS](k8s/dns/)

## Troubleshooting

* [Troubleshooting](k8s/debugging/)

## Helm

* [Deploy app using Helm](k8s/helm-app/)

## Extra excersises

* [Install Wordpress using Helm](https://github.com/bitnami/charts/tree/master/bitnami/wordpress) - Hint: Enable ingress using vars `ingress.*` and set `service.type` to `ClusterIp`
* Create an app deployment (e.g. nginx) from scratch  with a liveness probe, replicas, ingress, resource limits and configmap

## Katacoda

* [Network Introduction](https://www.katacoda.com/courses/kubernetes/networking-introduction)
* [Storage Introduction](https://www.katacoda.com/courses/kubernetes/storage-introduction)
* [Probes](https://www.katacoda.com/courses/kubernetes/liveness-readiness-healthchecks)
* [Ingress Routing](https://www.katacoda.com/courses/kubernetes/create-kubernetes-ingress-routes)
* [Helm](https://www.katacoda.com/courses/kubernetes/helm-package-manager)
* [CKAD scenario](https://www.katacoda.com/courses/kubernetes/first-steps-to-ckad-certification)
* [Docker Compose for Kubernetes](https://www.katacoda.com/courses/kubernetes/deploy-docker-compose-using-kompose)

## Unwind

```
unset KUBECONFIG
```
and / or revert to your old config backed-up earlier

```
cp ~/.kube/configBackup ~/.kube/config
```