# JFrog Pipelines on Kubernetes Helm Chart

**Technical Preview of [JFrog Pipelines](https://jfrog.com/pipelines/), available exclusively to JFrog Enterprise+ customers**

## Prerequisites Details

* Kubernetes 1.10+

## Chart Details
This chart will do the following:

- Deploy PostgreSQL (optionally with an external PostgreSQL instance)
- Deploy RabbitMQ
- Deploy Redis (optionally as an HA cluster)
- Deploy JFrog Pipelines micro-services with Kubernetes nodes (StatefulSets) to run jobs on.
  - **Note:** It is limited to **3** nodes for the technical preview version!!!

## Requirements
- A running Kubernetes cluster
  - Dynamic storage provisioning enabled
  - Default StorageClass set to allow services using the default StorageClass for persistent storage
- A running Artifactory
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) installed and setup to use the cluster
- [Helm](https://helm.sh/) installed and setup to use the cluster (helm init) or [Tillerless Helm v2](https://github.com/rimusz/helm-tiller)


## Install JFrog Pipelines

### Add JFrog Helm repository
Before installing JFrog helm charts, you need to add the [JFrog helm repository](https://charts.jfrog.io/) to your helm client
 ```console
helm repo add jfrog https://charts.jfrog.io
helm repo update
```

### Install Pipelines Chart
**Note:* Pipelines only have been properly tested on GKE with exposing URLs via ingress.

To test Pipelines please have ready:
- GKE cluster
- [Artifactory HA](https://hub.helm.sh/charts/jfrog/artifactory-ha) with Enterprise+ License
- [Artifactory Custom Base URL](https://www.jfrog.com/confluence/display/RTF/Configuring+Artifactory) e.g. 
```
Main URL https://artifactory.example.com
Custom Base URL: https://myartifactory.example.com/artifactory
```
- [Nginx-ingress controller](https://hub.helm.sh/charts/stable/nginx-ingress)
- [Cert-manager](https://hub.helm.sh/charts/jetstack/cert-manager) (optional) installed there or use your own TLS cert for your domain.

As you will need `values-ingress.yaml`, `values-ingress-passwords.yaml` and `values-ingress-external-secret.yaml` files in later steps, pull latest chart:
```console
helm fetch jfrog/pipelines --untar
```

Please **update**:
- URLs in `values-ingress.yaml`
- Passwords in `values-ingress-passwords.yaml`

Install JFrog Pipelines:
 ```console
helm upgrade --install pipelines --namespace pipelines jfrog/pipelines -f values-ingress.yaml -f values-ingress-passwords.yaml
```

### How to use external secret

**Note:** It is in best recommended practices to use external secrets instead of storing passwords in `values.yaml` files.

Fill in all required passwords in `values-ingress-passwords.yaml` and create and install the external secret:
```console
helm template --name pipelines-external-secrets pipelines/ -x templates/pipelines-secrets.yaml \
    -f values-ingress-passwords.yaml | kubectl apply --namespace pipelines -f -
```

Don't forget **update** URLs in `values-ingress-external-secret.yaml` file.
Install JFrog Pipelines:
 ```console
helm upgrade --install pipelines --namespace pipelines jfrog/pipelines -f values-ingress-external-secret.yaml
```

### Status
See the status of your deployed **helm** release:
 ```console
helm status pipelines
```

## Useful links
- https://www.jfrog.com/confluence/display/CICD/Getting+Started
- https://www.jfrog.com/confluence/display/CICD/Using+Pipelines
- https://www.jfrog.com/confluence/display/CICD/Managing+Runtimes
