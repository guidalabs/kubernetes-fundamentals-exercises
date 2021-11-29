# Create GKE cluster

Make sure you have a Google Cloud account.

For more info about creating GKE clusters see:
<https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster>

## Create project

Open the Google Cloud Console

Create a new project named `k8s-beyond'

https://console.cloud.google.com/projectcreate

## Enable Kubernetes Enginee API

https://console.cloud.google.com/kubernetes/

## Activate Cloud Shell

We will use the Google Cloud Shell so that we don 't have to install all necesary tools and have access to all Google APIs. 

## Create Cluster

Set the compute region to 'Eemshaven, Netherlands':

```bash
gcloud config set compute/region europe-west4
```

Create cluster with three nodes in different zones:

```bash
gcloud container clusters create k8s-beyond \
    --zone europe-west4-a \
    --node-locations europe-west4-a,europe-west4-b,europe-west4-c \
    --cluster-version "1.14.6-gke.13" \
    --machine-type "g1-small" \
    --preemptible \
    --num-nodes=1
```

## Get kubeconfig (optional)

```bash
gcloud container clusters get-credentials k8s-beyond --zone europe-west4-a
```

This will generate a kubeconfig file in `~/.kube/config`




