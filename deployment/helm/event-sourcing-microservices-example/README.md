# Deploying with Helm

Helm is a package manager for Kubernetes that can be used to deploy a distributed system. Each [Hyperskale](http://www.github.com/hyperskale) example application has a Helm package to help you get up and running with deploying a hyper-scalable distributed system example to Kubernetes as fast as possible.

This tutorial is for users who are wanting to deploy the hyper-scalable production version of our social network example to a remote Kubernetes cluster.



### Installing Helm

Please follow the instructions for installing Helm at [https://docs.helm.sh/using_helm/#installing-helm](https://docs.helm.sh/using_helm/#installing-helm).

### Adding Bitnami

Add the bitnami Helm repository which contains the `kafka` and `zookeeper` Helm charts required for this example.

```bash
helm repo add bitnami https://charts.bitnami.com
```

## Update Dependencies

Run a few helm commands to ensure all dependent charts are available:

```bash
helm dep update deployment/helm/event-sourcing-microservices-example
helm dep update deployment/helm/friend-service
helm dep update deployment/helm/user-service
helm dep update deployment/helm/recommendation-service

```

## Deploy

Once Helm is set up, deploying the distributed system to any Kubernetes cluster is quite simple.

```bash
helm install --namespace esme --name esme --set fullNameOverride=esme \
  deployment/helm/event-sourcing-microservices-example
```

There is no application-level security for the example app at this point, therefore rather than exposing the edge
to the internet we can utilize `kubectl port-forward` to access the app.

```bash
kubectl --namespace esme port-forward svc/edge-service 9000
```

Now, run the following script to test adding users and friend relationships to the social network.

```bash
sh ./deployment/sbin/generate-social-network.sh
```