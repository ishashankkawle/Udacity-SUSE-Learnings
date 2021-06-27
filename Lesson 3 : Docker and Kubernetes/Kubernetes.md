# Kubernetes
Kubernetes is container orchestration  framework which is used to manage multiple containers at scale

### Kubernetes vs Docker:
Docker is used to work on single or more container at a time but not at scale. It is used to create images of application and test out images in local environment. On other hand Kubernetes is used to manage containers at scale. It provides advance functionalities like heath management for containers , data persistency management and much more.

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


## Contributors:
1) Shashank Kawle : [LinkdIn](https://www.linkedin.com/in/ishashankkawle/) [Github](https://github.com/ishashankkawle)
