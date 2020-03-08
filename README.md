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
* Install kubectl on Ubuntu:
```
sudo snap install kubectl --classic
kubectl version
```

* Install virtualbox on Ubuntu:
```
sudo apt-get install virtualbox
```

* Install minikube on Ubuntu : 
https://kubernetes.io/docs/tasks/tools/install-minikube/
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube

sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/

minikube start --vm-driver=virtualbox

minikube status

minikube stop

```

* Start a minikube local

```
minikube start
```

* Start the minikube dashboard

```
minikube dashboard
```

Local cluster deployment
-------------------------

* deploy the hands-on-lab name space

```
kubectl apply -f namespace.yml
```

* deploy local Zookeeper single node cluster

```
kubectl apply -f zookeeper/local/zookeeper
```

* verify if Zookeeper installed correctly

 ```
kubectl get pods --namespace=hands-on-lab
# get the name of pod: zookeeper-deployment-785c9d99c8-pv8qw
kubectl exec zookeeper-deployment-785c9d99c8-pv8qw --namespace=hands-on-lab zkCli.sh create /hello world
kubectl exec zookeeper-deployment-785c9d99c8-pv8qw --namespace=hands-on-lab zkCli.sh get /hello
```

"world" should be seen in the end of output

* deploy local Kafka 3 nodes cluster

```
kubectl apply -f kafka/local/kafka.yml
```

* verify the cluster's event

```
kubectl get all --namespace=hands-on-lab
kubectl get events --namespace=hands-on-lab
```

* run interactive bash in one POD

```
kubectl exec -it kafka-0 --namespace=hands-on-lab /bin/bash
```

deploy local kafka client POD
-----------------------------

```
kubectl apply -f kafka/local/kafka-client.yml
```

Try create a topic into the Kafka cluster

```
kubectl exec -it kafka-client --namespace=hands-on-lab /bin/bash
kafka-topics --bootstrap-server kafka:9092 --create --replication-factor 3 --partitions=3 --topic test_topic
kafka-topics --bootstrap-server kafka:9092 --describe --topic test_topic
```

Have a look what the Kafka logs said

```
kubectl logs -f kafka-2 --namespace=hands-on-lab
```

Here kafka-2 is one of the kafka POD's name

Try some perf test like :

```
kafka-producer-perf-test  --topic test_topic --num-records 50000 --record-size 100 --throughput -1 --producer-props acks=-1 bootstrap.servers=kafka:9092 buffer.memory=67108864 batch.size=8196

kafka-console-consumer --bootstrap-server kafka:9092 --from-beginning --topic test_topic  --property print.key=true
```

