# JFrog Pipelines on Kubernetes Helm Chart

** Technical Preview of [JFrog Pipelines](https://jfrog.com/pipelines/), available exclusively to JFrog Enterprise+ customers**

## Prerequisites Details

* Kubernetes 1.9+

## Chart Details
This chart will do the following:

- Deploy PostgreSQL (optionally an external PostgreSQL instance)
- Deploy RabbitMQ
- Deploy Redis (optionally as an HA cluster)
- Deploy JFrog Pipelines micro-services with 3 Kubernetes nodes (StatefulSets) to run jobs on.
  - *Note:* It is limited to 3 nodes for the technical preview version!!!

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
```bash
helm repo add jfrog https://charts.jfrog.io
```

### Install Pipelines Chart
To test Pipelines please have ready:
- GKE cluster
- [Artifactory HA](https://hub.helm.sh/charts/jfrog/artifactory-ha) with Enterprise+ License
- [Artifactory Custom Base URL](https://www.jfrog.com/confluence/display/RTF/Configuring+Artifactory) e.g. 
```
Main URL https://artifactory.example.com
Custom Base URL: https://myartifactory.example.com/artifactory
```
- [Nginx-ingress controller](https://hub.helm.sh/charts/stable/nginx-ingress)
- [Cert-manager](https://hub.helm.sh/charts/jetstack/cert-manager) installed there or have TLS cert for your domain.

Install JFrog Pipelines with `values-ingress.yaml` file, please **update** passwords and URLs there.
```bash
helm upgrade --install pipelines --namespace pipelines jfrog/pipelines -f values-ingress.yaml
```

## Status
See the status of your deployed **helm** releases
```bash
helm status pipelines
```
The chart be default will install three Kubernetes nodes (deployments) to run jobs on, so you all set there.

## Useful links
- https://www.jfrog.com/confluence/display/CICD/Getting+Started
- https://www.jfrog.com/confluence/display/CICD/Using+Pipelines
- https://www.jfrog.com/confluence/display/CICD/Managing+Runtimes
