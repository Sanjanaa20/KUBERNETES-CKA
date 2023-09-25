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
1.Controller-manager
2.Node-Controller 
3.Replication-Controller

Communication:
The kube-apiserver is the primary management component of kubernetes responsible for orchestrating all operations within the cluster.
•We need these software that can run container and container runtime engine.

### Captain: Kubelet
A kubelet is an agent that runs on a each node in the cluster. It listens for instruction from the kube-apiserver and deploys or destroys containers on the node as required.
Kube-proxy service: Enables communication between webserver to database