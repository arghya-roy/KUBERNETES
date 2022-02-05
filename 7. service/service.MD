**Services:** 
```
         we have two nodes and one master. suppose we have a deployment, under that deployment we have a replica set and under that replica set a pod is running. 
in case iif that pod is destroyed then a new pod will be created automatically. That new pod can be created on same node as before or can be created in a new node.
For both of the cases the ip of new pod will be different from previous pod.

as a result of this we have to remember the ip of new pod when it will be created again and again. but as a solution of this problem
we can use a another object called services. by using service we can map a virtual ip with pod by using level and selector. After mapping with services and pod
if the old pod is deleted and afterthat new pod will be created, so the ip of pod will be changed. but automatically the ip of new pod maps with
the virtual ip. so we can access the pod by using the same virtual ip.

There are four types of services:
      1) clusterIP
			2) nodeport
			3) loadbalancer ( just like elb in aws )
			4) Headless
			
we have to use services above a object like deployment or replica set etc.

service > deployment > replica set > pod > container

loadbalancer > nodeport > clusterIP > deployment > replica set > pod > container


suppose we are applying a service on a deployment then we need to create two yaml file. one for service and one for deployment. then we will attatch them by
using label and selector.

by default service can run only between 30000 to 32767
```

---  
  
 cluster ip service 
--- 
**clusterIP:**
```
          by using clusterIP on a pod we can access that pod from different node also ( we can access that pod from anywhere of the cluster). but
by using clusterIP we can not access the pod from outside the cluster. to access the pod from outside of cluster we need to use nodeport.

suppose we have two pod in same node, then they can talk to each other by localhost:port. but if we have two pod in two different nodes
then they cannot talk to each other by using localhost, so a solution of this cases we have to attach two different cluster ip for
two different node. then they can talk to each other by using clusterip. 
```
```

# vi deployment1.yml
```
```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
```
```
# kubectl apply -f deployment1.yml		  
# kubectl get deploy
```


```
# vi service1.yml
```
```
kind: Service                             # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80                               # Containers port exposed
      targetPort: 80                     # Pods port
  selector:
    name: deployment                    # Apply this service to any pods which has the specific label
  type: ClusterIP                       # Specifies the service type i.e ClusterIP or NodePort

```
```
# kubectl apply -f service1.yml		  
```
```
# kubectl get svc
```
```
# curl clustep:port ( then we can access pod )
```

---  
  
 nodeport service 
 
---

**nodeport service:**
```
by using clusterIP we can attach a permanent ip to a pod and can access that pods from the cluster.
but if we want to access the pod from internet or outside the cluster then we need to use nodeport.

nodeport is just a port number from 30000 to 32767. when we define nodeport in service then a port is mapped with cluster ip ( we dont need to create
a clusterIP seperately). in google when we enter DNS of ec2 (node): port_of_nodeport then it goes to clusterip:port_of_pod and from clusterIP:port_of_pod it goes to 
pod. in this way we can access a pod by using nodeport.

DNS of ec2:port_of_nodeport > clusterip:port_of_pod > ip of pod
```
```
# vi deployment2.yml
```
```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
```
```
# kubectl apply -f deployment2.yml		  
# kubectl get deploy
```

```
# vi service2.yml
```
```
kind: Service                             # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80                               # Containers port exposed
      targetPort: 80                     # Pods port
  selector:
    name: deployment                    # Apply this service to any pods which has the specific label
  type: NodePort                       # Specifies the service type i.e ClusterIP or NodePort
```
```
# kubectl apply -f service1.yml		  

# kubectl get svc
it will show port of nodeport and clusterIP.
go to google
DNS of ec2:port_of_nodeport

```
