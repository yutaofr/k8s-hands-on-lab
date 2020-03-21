# Cluster Monitoring

## Cluster resource monitoring

### Metrics collection

The objective is to monitor the cluster resource usage, like CPU, memory, network, disk usage, etc of nodes, pods, and containers

Implemented by Using Prometheus + Graphana

#### Pre-request 

After the new namespace created, a "default" Service Account will be created, we could verify it by:

```
kubectl get serviceaccounts --ns hands-on-lab
```

We will find the information below:
```
NAME      SECRETS   AGE
default   1         7m50s
```

#### Deploy Prometheus

The Prometheus' config is created by the ConfigMap 

```
kubectl create -f prometheus/local/prometheus-config.yml
```

Then RBAC should be configured in order to allow the Prometheus retrieve the Kubenetes' metrics,
Meantime, the Prometheus will be deployed in it's own POD by

```
kubectl create -f prometheus/local/prometheus-deployment.yml
```
The Prometheus service start with the config defined by the ConfigMap earlier

In the end, expose the prometheus service by

```
kubectl create -f prometheus/local/prometheus-service.yml
``` 

The prometheus console is accessible by

```
minikube service prometheus --namespace=hands-on-lab
```
