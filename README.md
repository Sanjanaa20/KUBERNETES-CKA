# KUBERNETES-CKA Notes
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
**Replication controller helps us run multiple instances of single pod** `replicas:3`

*Why do we need labels?*
There could be hundreds of other pods in the cluster running different application.This is where labelling our pods during creation comes.

## Deployment 
The group of pods forms deployment and the pause,resume,modify etc call the changes are rolled out together and available in the kubernetes Deployment.

## Services 
Enable communication between various components within and outside the application where IP ranges from 30,000 to 32,767 externally to access any server from the pod network.eg: frontend --> backend
### Types
1. NodePort 
2. Cluster IP(Default service)
3. Load Balancer

```
service.yaml for single pod
apiVersion: V1
kind: Service
metadata: 
  name: myapp-service
  namespace: dev
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: frontend
```

- kubectl create -f service.yaml
- kubectl get services 
  (curl http://192.168.1.2:30008)
- kubectl describe svc kubernetes 

##### To summarise,whether the single pod on a single node or multiple node on multiple nodes the service is created exactly the same without additional steps.

### Namespaces:
To differentiate from one another pods.
- kubectl get pods --namespace=research, to create kubectl create ns dev
- kubectl get pods --namespace=kube-system
- kubectl get pods --all -namespace

Solutions to practice test for namespaces

1. <details>
   <summary>How many Namespaces exist on the system?</summary>

   ```
   kubectl get namespace
   ```

   Count the number of namespaces (if any)
   </details>

1. <details>
   <summary>How many pods exist in the research namespace?</summary>

   ```
   kubectl get pods --namespace=research
   ```

   Count the number of pods (if any)
   </details>

1. <details>
   <summary>Create a POD in the finance namespace.</summary>

   ```
   kubectl run redis --image=redis --namespace=finance
   ```
   </details>

1. <details>
   <summary>Which namespace has the blue pod in it?</summary>

   ```
   kubectl get pods --all-namespaces
   ```

   Examine the output.

   Or use `grep` to filter the results, knowing that `NAMESPACE` is the first result column

   ```
   kubectl get pods --all-namespaces | grep blue
   ```

   </details>

1. <details>
   <summary>Access the Blue web application using the link above your terminal!!</summary>

   Press the `blue-application-ui` button at the top of the terminal. Try the following:

   ```
   Host Name: db-service
   Host Port: 6379
   ```
   </details>

1. <details>
   <summary>What DNS name should the Blue application use to access the database db-service in its own namespace - marketing?</summary>

   > db-service

   To access services in the same namespace, only the host name part of the fully qualified domain name (FQDN) is required.

   </details>

1. <details>
   <summary>What DNS name should the Blue application use to access the database db-service in the dev namespace?</summary>

   Either FQDN

   > db-service.dev.svc.cluster.local

   Or, it is sufficient just to append the namespace name

   > db-service.dev

   </details>

## Infrastructure as a Code IAAC (types)
### Imperative: Step by step instruction
1. Provision a VM by the name 'webserver'
2. Install Nginx software on it
3. Edit configuration file to use port '8080'
4. Edit configuration file fo web path '/var/www/nginx'
5. Load web page from git repo
6. Start Nginx server

### Declarative: Direct approach giving final destination.
 ```
 VM: web-server
 Package: nginx
 port: 8080
 path: /var/www/nginx
 code: git repo
 ```
Orchestration tools like Ansible,Puppet,chef,Terraform fall into declarative approach.

**What if the object already exists?**
When you update object, we should make sure that the object exists first before running.
- Update: kubectl apply -f nginx.yaml

## Scheduling

If the pod is pending then there is no scheduler present. To schedule use Nodename like under spec: nodeName: node01 in yaml file.

### Taints and toleration 

Nodes are taint and the bugs are tolerants

```
spec: 
  tolerations: 
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```
- By imperative method it can be done by
###### kubectl taint nodes node1 app=blue:NoSchedule
### TYPES

|  | During Scheduling| During Execution |
| :---         |     :---:      |          ---: |
| 1   | Required | Ignored|
| 2 |Preferred |Ignored |
|3|Required| Required |
|4| Preferred | Required |

### Resource Limit:
To prevent from insufficient CPU storage and memory by allocating the resources.
- 1Gi(Gibibyte) - 1,073,741,824 bytes
- 1Mi(Mebibyte) - 1,048,576 bytes
- 1Ki(Kibibyte) - 1024 bytes

```
spec: 
  resources:
    requests: 
       memory: "1Gi"
       cpu: 1
    limits: 
       memory: "2Gi"
       cpu: 2
```
### Daemon Sets:
- Deploy multiple instances of pod.
- Ensure that one copy of pod is always present in all nodes in the cluster.
```
   kubectl get pods daemonset --all-namespaces
```

### Static Pods:
If we don't have API server, how does the kubelet creates pod,it does from the directory on the server designated to store information about pod.
```
/etc/kubernetes/manifests
```
- The pod created by the kubelet on its own without the intervention from the API server or rest of the k cluster components are known as static pods.

## MOCK EXAM FOR PRACTICE BY MM

  1. Apply below manifests:

     <details>
     
     ```
     apiVersion: v1
     kind: Pod
     metadata:
       creationTimestamp: null
       labels:
         run: nginx-pod
       name: nginx-pod
     spec:
       containers:
       - image: nginx:alpine
         name: nginx-pod
         resources: {}
       dnsPolicy: ClusterFirst
       restartPolicy: Always
     status: {}
     ```
     </details>

  2. Run below command which create a pod with labels:

     <details>
     
     ```
     kubectl run messaging --image=redis:alpine --labels=tier=msg
     ```
     </details>

 
  3. Run below command to create a namespace:
     
     <details>

     ```
     kubectl create namespace apx-x9984574
     ```
     </details>

  4. Use the below command which will redirect the o/p:

     <details>

     ```
     kubectl get nodes -o json > /opt/outputs/nodes-z3444kd9.json
     ```
     </details>

  5. Execute below command which will expose the pod on port 6379:

     <details>

     ```
     kubectl expose pod messaging --port=6379 --name messaging-service
     ```
     </details>

  6. Apply below manifests:

     <details>

      ```
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        creationTimestamp: null
        labels:
          app: hr-web-app
        name: hr-web-app
      spec:
        replicas: 2
        selector:
          matchLabels:
            app: hr-web-app
        strategy: {}
        template:
          metadata:
            creationTimestamp: null
            labels:
              app: hr-web-app
          spec:
            containers:
            - image: kodekloud/webapp-color
              name: webapp-color
              resources: {}
      status: {}
      ```
      
      In v1.19, we can add `--replicas` flag with `kubectl create deployment` command:
      ```
      kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas=2
      ```
     </details>

  7. To Create a static pod, copy it to the static pods directory. In this case, it is `/etc/kubernetes/manifests`. Apply below manifests:

     <details>

     ```
     apiVersion: v1
     kind: Pod
     metadata:
       creationTimestamp: null
       labels:
         run: static-busybox
       name: static-busybox
     spec:
       containers:
       - command:
         - sleep
         - "1000"
         image: busybox
         name: static-busybox
         resources: {}
       dnsPolicy: ClusterFirst
       restartPolicy: Always
     status: {}
     ```
     </details>

  8. Run below command to create a pod in namespace `finance`:

     <details>

     ```
     kubectl run temp-bus --image=redis:alpine -n finance
     ```
     </details>

  9. Run below command and troubleshoot step by step:

     <details>

     ```
     kubectl describe pod orange
     ```

     Export the running pod using below command and correct the spelling of the command **`sleeeep`** to **`sleep`** 

     ```
     kubectl get pod orange -o yaml > orange.yaml
     ```
   
     Delete the running Orange pod and recreate the pod using command.
     
     ```
     kubectl delete pod orange
     kubectl create -f orange.yaml
     ```
     </details>

  10. Apply below manifests:

      <details>

      ```
      apiVersion: v1
      kind: Service
      metadata:
        creationTimestamp: null
        labels:
          app: hr-web-app
        name: hr-web-app-service
      spec:
        ports:
        - port: 8080
          protocol: TCP
          targetPort: 8080
          nodePort: 30082
        selector:
          app: hr-web-app
        type: NodePort
      status:
        loadBalancer: {}
      ```
      </details>

  11. Run the below command to redirect the o/p:

      <details>

      ``` 
      kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
      ```
      </details>

  12. Apply the below manifest to create a PV:

      <details>
     
       ```
       apiVersion: v1
       kind: PersistentVolume
       metadata:
         name: pv-analytics
       spec:
         capacity:
           storage: 100Mi
         volumeMode: Filesystem
         accessModes:
           - ReadWriteMany
         hostPath:
             path: /pv/data-analytics
       ```
       </details>
       


   






