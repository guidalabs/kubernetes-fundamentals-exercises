# Secrets

Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing confidential information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or in a container image.

## Overview of Secrets

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in an image. Users can create secrets and the system also creates some secrets.

To use a secret, a Pod needs to reference the secret. A secret can be used with a Pod in three ways:

* As files in a volume mounted on one or more of its containers.
* As container environment variable.
* By the kubelet when pulling images for the Pod.

## Define secret as environment variable

Creating secret:

```bash
cat secret-1.yaml
kubectl apply -f secret-1.yaml
```

Decoding secret:

```bash
kubectl get secret secret-1 -o yaml
echo 'MWYyZDFlMmU2N2Rm' | base64 --decode
```

Creating pod:

```bash
cat secret-1-pod.yaml
kubectl apply -f secret-1-pod.yaml
```

View result:

```bash
kubectl logs secret-1-test-pod
```

## Define secret as config file

Creating secret:

```bash
cat secret-2.yaml
kubectl apply -f secret-2.yaml
```

Creating pod:

```bash
cat secret-2-pod.yaml
kubectl apply -f secret-2-pod.yaml
```

View result after a fews seconds:

```bash
kubectl logs secret-2-test-pod
```

You can find more information and use cases:
<https://kubernetes.io/docs/concepts/configuration/secret/#overview-of-secrets>

## Clean up

```bash
kubectl delete -f secret-1-pod.yaml
kubectl delete -f secret-2-pod.yaml
kubectl delete -f secret-1.yaml
kubectl delete -f secret-2.yaml
```
