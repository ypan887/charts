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
 ```bash
helm repo add jfrog https://charts.jfrog.io
helm repo update
```

### Install Pipelines Chart
**Note:** Pipelines has only been properly tested on [Google Cloud GKE](https://cloud.google.com/kubernetes-engine/) with ingress.

It should work with ingress on [AWS EKS](https://aws.amazon.com/eks/) and [Azure AKS](https://azure.microsoft.com/en-gb/services/kubernetes-service/) as well.

#### Pre-requisites 
Before deploying Pipelines you need to have the following
- A Google [GKE](https://cloud.google.com/kubernetes-engine/) cluster
- An [Artifactory HA](https://hub.helm.sh/charts/jfrog/artifactory-ha) with Enterprise+ License
- The [Artifactory Base URL](https://www.jfrog.com/confluence/display/RTF/Configuring+Artifactory). For example:
```
Artifactory URL: https://artifactory.example.com
Artifactory Base URL: https://artifactory.example.com/artifactory
```
- A static (external) IP in your GCP account to simplify connectivity with Artifactory. See [GCP guide](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address)
- Deployed [Nginx-ingress controller](https://hub.helm.sh/charts/stable/nginx-ingress)
- [Optional] Deployed [Cert-manager](https://hub.helm.sh/charts/jetstack/cert-manager) for automatic management of TLS certificates with [Lets Encrypt](https://letsencrypt.org/)
- [Optional] TLS secret needed for https access

#### Prepare configurations
Fetch the JFrog Pipelines helm chart to ge the needed configuration files
```bash
helm fetch jfrog/pipelines --untar
```

Edit local copies of `values-ingress.yaml`, `values-ingress-passwords.yaml` and `values-ingress-external-secret.yaml` with the needed configuration values 

- URLs in `values-ingress.yaml`
  - Artifactory URL
  - Ingress hosts
  - Ingress tls secrets
- Passwords in `values-ingress-passwords.yaml`

#### Install JFrog Pipelines
Install JFrog Pipelines
 ```bash
helm upgrade --install pipelines --namespace pipelines jfrog/pipelines -f values-ingress.yaml -f values-ingress-passwords.yaml
```

### Use external secret
**Note:** Best practice is to use external secrets instead of storing passwords in `values.yaml` files.

Fill in all required passwords in `values-ingress-passwords.yaml` and create and install the external secret:
```bash
helm template --name pipelines-external-secrets pipelines/ -x templates/pipelines-secrets.yaml \
    -f values-ingress-passwords.yaml | kubectl apply --namespace pipelines -f -
```

Don't forget to **update** URLs in `values-ingress-external-secret.yaml` file.

Install JFrog Pipelines:
 ```bash
helm upgrade --install pipelines --namespace pipelines jfrog/pipelines -f values-ingress-external-secret.yaml
```

### Status
See the status of deployed **helm** release:
 ```bash
helm status pipelines
```

## Useful links
- https://www.jfrog.com/confluence/display/CICD/Getting+Started
- https://www.jfrog.com/confluence/display/CICD/Using+Pipelines
- https://www.jfrog.com/confluence/display/CICD/Managing+Runtimes
