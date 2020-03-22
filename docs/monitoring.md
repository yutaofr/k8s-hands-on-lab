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

#### Deploy Grafana

Deploy the Grafana service by

```
kubectl create -f grafana/local/grafana-deployment.yml
kubectl create -f grafana/local/grafana-service.yml
```

The Grafana console is accessible via

```
minikube service grafana --namespace=hands-on-lab
```

##### Grafana's dashboard Config 

1. Username is `admin` and password is also `admin`.

1. Add `Prometheus` as a datasource.

* Click on the icon `Data Sources`.
* Click `Add data source`.
* Use `prometheus` as Data Source Name
* Select `Prometheus` as the type
* For the URL, we will actual use Kubernetes DNS service discovery. 
    * Enter `http://prometheus:9090`. Grafana will lookup the `prometheus` service 
    running in the same namespace as it on port `9090`.

1. Create a New dashboard 

* Via the icon Dashboard->New. Click the green control and add a graph panel. 
* Under metrics, select `prometheus` as the datasource. 
* For the query, use `sum(container_memory_usage_bytes) by (pod_name)`. 
* Click save. This graphs the memory used per pod.
