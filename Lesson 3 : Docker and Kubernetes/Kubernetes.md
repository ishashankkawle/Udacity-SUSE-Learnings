# Kubernetes
Kubernetes is container orchestration  framework which is used to manage multiple containers at scale

### Kubernetes vs Docker:
Docker is used to work on single or more container at a time but not at scale. It is used to create images of application and test out images in local environment. On other hand Kubernetes is used to manage containers at scale. It provides advance functionalities like heath management for containers , data persistency management and much more.

### Deployment and Replica Set
Deployment is newer and higher level concept. It allows deployment of Replica Set and allows easy updating of Replica Set.
Replica Set maintains stable set of pods running at a time. It also guarantee the availability of specified number of identical pods. Replica Sets are similar to Replication Controller which is older version to Replica Set.

### Basic Command Structure for kubectl:
``` kubectl <action> <component> [options]```

### To view resources in cluster:
Use ```kubectl get``` command. Some the commonly used get statements are :
##### 1) To get the details of cluster & health status: ( MOVE TO HEALTH AND STATUS SECTION )
``` kubectl cluster-info``` 
##### 2) To get  the list of namespaces:
``` kubectl get ns``` 
##### 3) To get the list of nodes in cluster:
``` kubectl get nodes``` 
##### 4) To get the list of deployments:
``` kubectl get deploy``` 
##### 5) To get the list of pods:
``` kubectl get pod``` 
##### 6) To get the list of services:
``` kubectl get svc``` 
##### 7) To get the list of persistent volumes:
``` kubectl get pv``` 
##### 8) To get the list of persistent volume claims:
``` kubectl get pvc```
##### 9) To get the list config maps:
``` kubectl get cm``` 
##### 10) To get the list of secrets:
``` kubectl get secrets```  
##### 11) To get all Running components of kubernetes:
```kubectl get all```

 If you want to narrow down search in specific namespace or want to perform any kind of operation in particular namespace, you can pass namespace in any kubectl command using ```-n``` flag. 
Example: To get all pods from demo namespace
```kubectl get pods -n demo```

To get more detailed output you can pass ```-o wide``` option with kubectl get  command
Example: ```kubectl get pods -o wide```
 
 You can also select any resources based on labels assigned to them using ```--selector``` option.
 
 Example: To get all pods with label **webapp1**
 ```kubectl get pods --selector="name=webapp1"```

##### 12) To get all events:
Kubernetes also maintains history of events. Use below command to get history of all events.
```kubectl get events```

### To get more detailed Information
To get more detailed information regarding any component, use ```kubectl describe``` command

Syntax: ```kubectl describe <component-type> <component-name>```

Example:
##### To get more information about pod 'my-pod'
```kubectl describe pod 'my-pod'```

##### To get more information about service 'my-nodeport-service'
```kubectl describe pod 'my-nodeport-service'```

### To get cluster - level Information
Similarly, you can check cluster level information with ```kubectl cluster-info``` command

Syntax: ```kubectl cluster-info```
Also, to get deeper details of each resources in cluster you can generate complete dump of cluster

```kubectl cluster-info dump --all-namespaces --output-directory=my_dir --output=json```

This will export all the artefacts of cluster including all the namespaces , pods , nodes,services etc. in json format into my_dir folder 

### To execute any command on pod:
To execute any command on pod use ```kubectl exec``` wit ```-it``` flag is referred to interactive terminal in short form.

Syntax: ```kubectl exec -it <pod name> <command>```

Example :
```kubectl exec -it my-pod ls```

This can also be used to open interactive terminal of pod as follows:
```kubectl exec -it my-pod /bin/sh```
where ```/bin/sh``` is the command to open terminal
 
### To create new resources in cluster:
The commands that are used to create new resources in cluster are :

 1. ```kubectl create```
 2. ```kubectl run```
 
 ```kubectl create``` is used to create variety of objects with default initial values.
 ```kubectl run``` will create as well as perform operations with those objects.

There are two approaches when it comes to creating / modifying resources.

 - **Imperative**
 In this approach we use kubectl commands to quickly create/run/update resources. This approach is best suited for development environment where resource configurations get changed /updated frequently
 - **Declarative**
 In this approach we use ```yaml``` formatted files to create resources in declarative format. This approach is used while deploying resources in production environment where we want to keep the configuration consistent across nodes.

##### To create new resources ( IMPERATIVE )
1) Using **kubectl create**:
```kubectl create <component> [options]```
Example: To create deployment called my-deployment
```kubectl create deployment my-deployment ---image=nginx:alpine -n demo```
Where ,
```--image``` : base image used to create container
```-n``` : defines namespace where you want to create new deployment
> **Note:** we **cannot** provide port mapping with create command with ```--port``` option. This option is invalid in create command.

2) Using **kubectl run**:
```kubectl run<component> [options]```
Example: To create deployment called my-deployment
```kubectl run deployment my-deployment ---image=nginx:alpine -n demo --replicas=3```
Where ,
```--image``` : base image used to create container
```-n``` : defines namespace where you want to create new deployment
```--replicas```: guarantees that at least specified number of pods will be running at any instance of time. If any pod get crashed, new pod will get created.

##### To create new resources ( DECLARATIVE )

There are two commands used to create resources using yaml files
Lets assume we have abc.yaml file which contains information about deployment
To deploy this deployment :
 - kubectl create:  ``` kubectl create -f abc.yaml```
 - kubectl apply: ```kubectl apply -f abc.yaml```
in both cases ```-f``` flag is used to provide path to the yaml file

But what is the difference between both commands?
```kubectl create``` only creates new resources
```kubectl apply``` create or updates resources if it already exists.


Now that we know how to create and deploy artefacts, this brings us to our next topic.

### To expose resources in cluster:

We can use ```kubectl expose``` or ```kubectl port-forward``` commands to expose ports.
However ```kubectl port-forward``` works on pod level only & generally used for debugging perpose for pod level operation.

```kubectl expose``` is used to expose ports for different components. Whenever we use ```kubectl expose``` command it automatically creates a sservice & exposes a static port to internet. All incomming calls are handled by this service.

```kubectl expose deploy my-deploy --external-ip="172.17.0.12" --port=8080 --target-port=80```
Where, 
```--external-ip``` = Ip which you want to expose for accessing the application
```--port``` = This is the port on which service is receiving incoming request
```--target-port``` = This is the port on which pods will receive the call from service

##### To verify:
Use curl command with binded ip:
```curl http://172.17.0.12:8080```

### Run and Expose deployment through Single Command:
With **kubectl run** it is possible to create deployment & expose at a same time.

We can use ```--hostport``` option with kubectl to expose deployment.

Example:
 ```kubectl run httpexposed --image=nginx-alpine --replicas=3 --port=80 --hostport=8001```

You can access the application via curl as below.
 ```curl http://127.0.0.1:8001```

Under the cover this exposes port via **docker port mapping** and not by creating handler service. Docker port mapping is nothing but exposing port via ```-p``` option.
Example:

```docker run -d -p 8080:80 my-image```

As a result, you will not se service listed using : ```kubectl get svc``` command.

To find more details about mapping , you can execute below command
 ```docker ps | grep httpexposed```

## Kubernetes Services
### Service types:
##### 1) ClusterIp : 
Exposes a static port internally in the cluster. This type of service is only reachable from within th cluster. This is the DEFAULT type for service.

##### 2) NodePort :
Exposes service on each nodes ip at static port 'NodePort'. A clusterIp service at each node to which NodePort service routes is automatically created.

##### 3) Load Balaner:
Exposes service externally using cloud providers load balancer. NodePort & ClusterIp services to which external loadbalancer routes are automatically created.

When pods have same labels a label selector is used in service to bind pods with service. All these pods will automatically get load balanced behind that service.

Below is the sample **deployment.yaml** to deploy the pods.
```yaml
apiVersion: V1
kind: Deployment
spec:
 replicas: 4
 selector:
   matchlabels:
     app: webapp1
template:
 metadata:
   labels:
     app: webapp1
 spec:
   containers:
     - name: webapp1
       image: nginx:alpine
       ports:
         - containerPort: 80
```
Below is sample **service.yaml** file to create service which will expose pods.

```yaml
apiVersion: V1
kind: Service
metadata:
  name: web-svc
  labels: 
    app: webapp1
  spec:
    type: NodePort
    ports:
      -	port: 80
        nodePort: 30080
    selector:
      app: webapp1
```

In above service.yaml file ``` -	port``` is the container port where service will redirect the request.
```nodePort``` is the exposed port which we use as below:
```curl localhost:3000```

If we want to conect service to any pods we have to add below keywords in its yaml file.
```yaml
selector:   #Defines how to select pods
 app: webapp1 #webapp1 is the label which is assigneed to pods
```
We can interprete this like select all pods with label **webapp1**

### ClusterIp Service:
The format for ClusterIp service yaml file is as below:
```yaml
apiVersion: V1
kind: Service
metadata:
  name: cluster-service
  labels:
    app: cluster-svc-1
spec:
  ports:
    -	port: 8080
      targetPort: 80
  selector:
    app: webapp1
```
 **targetPort** is port which application is configured o listen on. **Port** is how application will be accessed from outside but from within the cluster.

### NodePort Service:
While CluterIp service exposes nodes ip inside the cluster, NodePort service exposes nodes ips outside of cluster via static port.

```yaml
apiVersion: V1
kind: Service
metadata:
  name: Node-service
  labels:
    app: Node-svc-1
spec:
  type: NodePort
  ports:
    -	port: 80
      nodePort: 30080
  selector:
    app: webapp1
```
In NodePort type of service if you didn't assign **nodePort** value, kubernetes will assign available port randomly.
To find out which port got assigned, use below command:
```kubectl describe svc Node-service | grep NodePort```
 
### Exposing Service via External-Ip:
Another approach to make a service available outside the cluster is via External-Ip address.

```yaml
apiVersion: V1
kind: Service
metadata:
  name: extip-service
  labels:
    app: ext-svc-1
spec:
  ports:
    -	port: 80
  externalIps:
    -	172.17.0.18
  selector:
    app: webapp1
```
This service is now bounded to external ip and port 80 of master node.

## Persistent Volumes
Pods can read/write data in/from persistent volumes by mounting specific directory on pod with host/nfs server.
For learning purpose we are using host to store data. For production we use protocols like nfs which exposes directory from master node to pods for data persistency.
#### 3 Artefacts:
1. Persistent Volume
2. Persistent Volume Claim
3. Pod to mount directory

#### Reclaim Policies:
##### Delete: 
Persistent Volume will be deleted when Personal Volume Claim is deleted but data will persist.

##### Recycle:
Volume contents will be deleted. Persistent Volume will be available to be claimed again

##### Retain:
This is DEFAULT policy. Persistent Volume will be retained with all data. even after claim mapped to it is deleted. Volume will be available again for new claim to be mapped.

#### Access Modes:
##### ReadWriteOnce:
Only Single node can mount for Read / write.

##### ReadWriteMany:
Multiple nodes can mount for Read / write.

##### ReadOnlyMany:
Multiple nodes can mount for read only.

####  1) Deploy Persistent Volume 
It defines space to store data. When container has finished with the volume, data can be **Retained** for future use or **Recycled** meaning all data will be deleted. This is defined by ```persistentVolumeReclaimPolicy``` option
Sample **persistentVolume.yaml** is given below:
```yaml
apiVersion: V1
kind: PersistantVolume
metadata:
 name: <friendly name>
spec: 
 capacity:
  storage: 5Gi
 accessModes:
  - ReadWriteOnce
  - ReadWriteMany
 persistentVolumeReclaimPolicy: Recycle
 hostPath:
  path: /home/my_dir/vol
```
Where ```hostPath``` is used for mounting directories from worker node's file system into pod.
Whenever we are using nfs, then instead of hostPath we provide nfs configuration as below.

```yaml
nfs:
 server: <server-name>
 path: <shared-path>
```
To view all Persistent Volumes:

```kubectl get pv``` 

#### 2) Deploy Persistent Volume Claim
Claim is designed to stop apps accidently writing to other volume resulting in data corruption. Claim specifies requirement for volume. This includes read/ write access & storage space required.

Sample **persistentVolumeClaim.yaml** is given below:
```yaml
apiVersion: V1
kind: PersistentVolumeClaim
metadata:
 name: my-claim
spec:
 accessMode:
  - ReadWriteOnce
 resources:
  request:
   storage: 3Gi
```

Claims will get mapped with Persistent volume automatically which satisfy storage quota.
To view all Persistent Volume Claims:

```kubectl get pvc``` 

>**Note:** If PVC doesn't find any Persistent Volume that meets quota, it will remain in*pending* state until volume is available. 
>Similarly, if PVC is not assigned to any volume, then pod will remain in*pending* state until it became available.


#### 3) Deploy the POD
We are going to map */var/lib/mydata* folder to */home/my_dir/vol* folder defined for Persistent Volume.
What this mean is */var/lib/mydata* & */home/my_dir/vol* will be in SYNC and will act as a single directory.
Sample **pod.yaml** is given below:
```yaml
apiVersion: V1
kind: Pod
metadata:
 name: my-pod
 labels:
  name: pod1
spec:
 containers:
  - name: con1
   image: nginx:alpine
 ports:
  - containerPort: 80
 volumeMounts:
  - name: <some friendly name>
  mountPath: /var/lib/mydata
volumes:
 - name: <friendly name>
 persistentVolumeClaim:
  clainName: my-claim
```

#### 4) Verify Read / Write
From host machine, create one file in */home/my_dir/vol* directory.
```bash
cd /home/my_dir/vol
echo "Hello World" > hello.txt
```
Now open the interactive terminal of pod & navigate into mounted directory. hello.txt should be present there.
To open interactive terminal of pod, use below command:
```kubectl exec -it my-pod /bin/sh```

Through interactive terminal of pod, navigate to */var/lib/mydata* and verify if hello.txt is present there.
```bash
cd /var/lib/mydata
ls
cat hello.txt
```

## Scale Containers:
To scale up / scale down containers of a cluster , use **kubectl scale** command:

Syntax: ```kubectl scale --replicas=3 deployment my-deploy```

If replica count is greater that current number of pods, kubernetes will spin-up more containers to match the count. If it is less than current number of pods, kubernetes will shut down some pods to match the replica count. 

## Contributors:
1) Shashank Kawle : [LinkdIn](https://www.linkedin.com/in/ishashankkawle/) [Github](https://github.com/ishashankkawle)
