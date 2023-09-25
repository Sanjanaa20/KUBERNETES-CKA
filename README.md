# KUBERNETES-CKA
Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications. It is now maintained by the Cloud Native Computing Foundation.  This Kubernetes certification course helps you gain the knowledge required to design and deploy cloud native applications on a Kubernetes cluster. 

## Purpose
- To host your application in the form of containers in an automated fashion.
- We can easily deploy as many instaces of your application as required ans easily enable communication.

### Node
The master node in the Kubernetes cluster the master node is responsible for managing the Kubernetes.
Master-> Manage,Plan,Schedule,Monitor nodes
Control Node-> Host application as containers

## Real life example
1. We will look at each of these components and now there arr many containers being loaded and unloaded from the ships daily.
2. As you need to maintain information about different ships what container is on which ship and what time it was loaded etc
3. All of these are stored in a highly available key value store known as ETCD

### Schedulers 
Scheduler in a kubernetes cluster  identifies as the right node to place a container in the right place by kube-scheduler
Controllers:
1.Controller-manager.
2.Node-Controller.
3.Replication-Controller.

Communication:
The kube-apiserver is the primary management component of kubernetes responsible for orchestrating all operations within the cluster.
•We need these software that can run container and container runtime engine.

## Captain of the ship: Kubelet
A kubelet is an agent that runs on a each node in the cluster. It listens for instruction from the kube-apiserver and deploys or destroys containers on the node as required.
>kubectl run --help

Kube-proxy service: Enables communication between webserver to database.

**ETCD** is a distributed reliable key-value store that is simple,secure and fast.
<key-value store>

| Name | Age| Location|
| :---  | :---: |    ---: |
| John   | 45    | Newyork    |
| Sanju    | 20       | India |
- stores information in document or pages.
 **When your data gets complex ends up storing JSON**

```
{
"name"="Sanju"
"age"=20
"location"="India"
"salary"=10,000,00
}
```
### Kube Controller Manager
A controller is a process that continuously monitors the state of various components within the system.
 
### Node Controller
It monitors the health of the nodes if it stops receiving heartbeat from a node,it is marked as unreachable and waits for 5mins then removed and creates another one from the pod.
#### Node monitor period-5s
#### Node monitor grace period-40s
#### POD eviction timeout-5m

### Kube scheduler 
-which pod goes on where 
-right container ends up right ship

A **pod network** is an interval VN that spans across all the nodes in the cluster to which all the PODS connect.
eg: web application deployed on the first node and the database application deployed-connect using IP of the database of POD.

## POD
Kubernetes does not deploy containers directly on the worker nodes.The containers are encapsulated into a kubernetes object known as **Pod** which is a single instance of an application.A single pod can have multiple containers.

### YAML
Kubernetes definition file always contains four top level fields.

```
apiVersion: V1
kind: Pod
metadata: 
  name: myapp
  labels:
    app: myapp-pod
    type: front-end
spec: 
  containers:
    -name: nginx container
     image: nginx
```
To create 
• kubectl create -f <filename>.yaml
• kubectl get pods

## Replication controller: ReplicaSet

When the application crashed and pod fails,users will no longer be able to access yoour application.To prevent users from losing access,to our application we would like to have more than one instance or pod running at the same time.If one fails,we still have one application.
** Replication controller helps us run multiple instances of single pod ** `replicas:3`

* Why do we need labels? *
There could be hundreds of other pods in the cluster running different application.This is where labelling our pods during creation comes.

## Deployment 
The group of pods forms deployment and the pause,resume,modify etc call the changes are rolled out together and available in the kubernetes Deployment.
