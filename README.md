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
* [] VPC
* [] subnet
* [] communications
* [] zookeeper cluster
* [] kafka cluster
* [] deploy applications kafka stream (with spring-boot delivered with container)
