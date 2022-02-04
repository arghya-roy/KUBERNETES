##### pod manages container.
##### replica set manages pod.
##### pod have a different private IP. container dont have any ip.
##### suppose we a have a master and 3 nodes. when we create a pod in master then by default master can creates this pod any any of the nodes. If 
##### the pod fails then we cant recover the same pod but we can configure to get a exact same pod with different unique id.


##### when we use kind: pod then if this pod is deleted then it can never by created automatically. to create it automatically we will leran it later.

**to apply pod
```
# kubectl apply f pod_name.yml
```
to see all pods
```
# kubectl get pods
```
to see that which pod is running on which nodes. it also shows pvt ip of pods.
```
# kubectl get pods -0 wide
```
to see details of a single pod
```
# kubectl describe pod pod_name
       or
# kubectl describe pod/pod_name
```
to see that what is happening in container. if pod has multiple container then we have to specify container name.
```
# kubectl logs -f pod_name
```
if have multiple container
```
# kubectl logs -f pod_name -c container_name
```

to delete pod
```
# kubectl delete pod pod_name
# kubectl delete -f name.yml	   
```
---
                                                                                pod
---
{ kind of a pod is always "pod". apiVersion of a pod is always v1. under metadata the name is name of the pod.
incase of pod1 we create one container. name of that container is c00 and image is ubuntu. after creation of that container
it will run /bin/bash command in container. -C means container. while true; do echo hello-bhupinder means if container creation succesfull
the run hello-bhupinder for infinite time }

---

```
#vi pod1.yml
```
```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod                  
spec:                                    
  containers:                      
    - name: c00                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
  restartPolicy: Never         # Defaults to Always
```
```
#kubectl apply -f pod1.yml
```
```
#kubectl get pods
```
in output it shows testpod has 1/1 that means we have only one container in pod.
```
# kubectl get pods -o wide (it also shows pvt ip of pods)
```
```
# kubectl describe pod testpod
```
```
# kubectl logs -f testpod   ( it shows hello-bhupinder )
```
```
# kubectl delete pod testpod
```
```
# kubectl delete -f pod1.yml
```

---
if pod have multiple container
```
vi pod2.yml
```
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod2
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    - name: c01
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
	  
```
```
#kubectl apply -f pod2.yml
```
```
#kubectl get pods
```
in output it shows testpod3 has 2/2 that means we have only one container in pod.
```
# kubectl get pods -o wide (it also shows pvt ip of pods)
# kubectl describe pod testpod2   
# kubectl logs -f testpod2 ( it gives a error but shows all container_name of pod )  
# kubectl logs -f testpod2 -c c00   ( it shows hello-bhupinder )
# kubectl logs -f testpod2 -c c01   ( it shows hello-bhupinder )
```
to go inside container
```
# kubectl exec testpod3 -it -c c01 -- /bin/bash
          inside container
		  # ps 
		  # ps -ef
```
```
# kubectl delete pod testpod2
```
```
# kubectl delete -f pod2.yml
```

---
annotation - under it we write that why we create this pod. when we use kubectl describe pod pod_name then we can see annotation.
we write annotation in metadata section.
```
#vi pod3.yml
```
```
kind: Pod                              
apiVersion: v1                     
metadata:                           
  name: testpod3
  annotations:
    description: I create this pod because of...  
spec:                                    
  containers:                      
    - name: c00                     
      image: ubuntu              
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
  restartPolicy: Never         # Defaults to Always
  ```
  ```
#kubectl apply -f pod3.yml
#kubectl get pods
in output it shows testpod3 has 1/1 that means we have only one container in pod.
# kubectl get pods -o wide (it also shows pvt ip of pods)
# kubectl describe pod testpod3 (here we can see annotation section means we can see the text which is written here)
# kubectl logs -f testpod3   ( it shows hello-bhupinder )
# kubectl delete pod testpod3
# kubectl delete -f pod3.yml
```
---
Enviroment variable - it is a key value pair. we need to define this in manifest file. after applying this file if we go inside the pod and run echo $key then it
returns value.
```
# vi pod4.yml
```
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod4
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
      env:                        # List of environment variables to be used inside the pod
      - name: MYNAME
        value: BHUPINDER
```
```
#kubectl apply -f pod4.yml
#kubectl get pods
in output it shows testpod4 has 1/1 that means we have only one container in pod.
# kubectl get pods -o wide (it also shows pvt ip of pods)
# kubectl describe pod testpod4   
# kubectl logs -f testpod4  
# kubectl exec testpod3 -it -c c00 -- /bin/bash
          or
# kubectl exec testpod3 -it -- /bin/bash

          inside container
		  # env ( to go inside env)
		  # echo $MYNAME
```
```
# kubectl delete pod testpod4
# kubectl delete -f pod4.yml	

```

---

POD WITH PORTS- to expose port in pod
```
#vi pod5
```
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod4
spec:
  containers:
    - name: c00
      image: httpd
      ports:
       - containerPort: 80  
```
```
#kubectl apply -f pod5.yml
#kubectl get pods
in output it shows testpod5 has 1/1 that means we have only one container in pod.
# kubectl get pods -o wide (it also shows pvt ip of pods)
# curl ip of pod:port
  <it works>
# kubectl delete pod testpod5
# kubectl delete -f pod5.yml
```
---	

LABELS - used to identify pod
         supposed a application named arghya is running in 3 pod. just to identify that 3 pod we can use labels.
		 we can search them by label.
		 a label is a key value pair without any predefined meaning that can be attached to object.
		 Name(key): arghya(value)
		 we can give any text as key and value
		 label is same as tag in aws.
		 multiple label can be added to a single object.
		 label should be written under metadata.
		 many object can carry same label.
		 
labels selector - it is a process by using it we can select/get/delete pods based on its label.
once label are attached to object(pod), we need to filters them based on label. yhis is called label selector.
The api currently supports two types of selector -
                      1) equity based  key=value or key!=value ( it gives all pods which have this label)
					  2) set based     
					       a) key in(value1,value2)
						      it gives those pods contains key=any of these values
						   b) key notin(value1,value2)
						      it gives those pods which do not contains key=any of these values
						   c) exist
							  
		 
```
#vi pod6.yml
```
```
kind: Pod
apiVersion: v1
metadata:
  name: delhipod
  labels:                                                   
    env: development  
    class: pods
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
		 
```
```
#kubectl apply -f pod6.yml
#kubectl get pods
in output it shows delhipod has 1/1 that means we have only one container in pod.
# kubectl get pods -o wide (it also shows pvt ip of pods)
# kubectl describe pod delhipod
# kubectl logs -f delhipod   ( it shows hello-bhupinder )

to check labels on all pod
# kubectl get pods --show-labels

# kubectl delete pod delhipodpod
# kubectl delete -f pod6.yml

if you want to add label on existing pod
# kubectl label pods pod_name myname=arghya	
# kubectl get pods --show-labels
```

selector-

equity based- 

we can get list of pods which match a label-
```
# kubectl get pods -l env=development
```
we can get list of pods which does not match a label-
```
# kubectl get pods -l myname!=arghya
```
it gives those pods which contains all of these label -
```
# kubectl get pods -l env=development,myname=arghya
```
we can delete list of pods which match a label-
```
# kubectl delete pods -l env=development
```
we can delete list of pods which does not match a label-
```
# kubectl delete pods -l myname!=arghya
```
set based- 

it gives those pods which contains any of this labels-
```
# kubectl get pods -l 'key in(value1, value2)'
```
it gives those pods which do not contains any of this labels-
```
# kubectl get pods -l 'key notin(value1, value2)'
```
it deletes those pods which contains any of this labels-
```
# kubectl delete pods -l 'key in(value1, value2)'
```
it deletes those pods which do not contains any of this labels-
```
# kubectl delete pods -l 'key notin(value1, value2)'
```

	 




***************************************************************************
NODE SELECTOR - suppose we have one master and 4 nodes. when we apply a pod then master can apply that pod in any of the node.
                but if we want to secify a particular node where our is going to apply then we should use node selector.
				before that we have to attach a label on that particular node and then give that label under nodeSelector in 
				manifest file.
				
				Here i am taking a example of hardware node selector example.
				here below pod is going to apply that node which has hardware=t2-medium label.
				
				before doing that we have a give a label in node as hardware=2.medium (key=value)
				
				     here a question can arises that what happen if both we have multiple node hardware=t2-medium label?
				     in that case in which node master get free space at first, there this pod will be applied.
```					 
# kubectl label nodes ip... hardware=t2-medium					 
```
```
# vi pod7				 
```
```
kind: Pod
apiVersion: v1
metadata:
  name: nodelabelspod
  labels:
    env: development
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
    nodeSelector:                                         
       hardware: t2-medium
```	   
	   
```	   
#kubectl apply -f pod6.yml
#kubectl get pods
```
*****************************************************************************************************
