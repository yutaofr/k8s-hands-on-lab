# k8s-hands-on-lab
k8s hand on lab

Architecture:
![Architecture](k8s-test.bmp)

Network: one VPC, one subnet / availble zone
VM: one node / subnet
Communication: all flows open between the three nodes

Middlewares: one cluster zookeeper, one cluster Kafka with replicator = 2 (min.isr = 2), applications will be deployed as POD?

RoadMap
* [] Init k8s scripts

```
kubectl create namespace kafka
kubectl get namespaces
```

* [] VPC
* [] subnet
* [] communications
* [] zookeeper cluster

zookeeper cluster should be stateful: StatefulSet, persistentVolume
one port for broadcast, one port for Leader-election

https://kubernetes.io/docs/tutorials/stateful-application/zookeeper/


* [] kafka cluster
* [] deploy applications kafka stream (with spring-boot delivered with container)


Install notes:
--------------
* Install Kubernetes on Ubuntu : https://ubuntu.com/kubernetes/install

