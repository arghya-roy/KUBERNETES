**communication between two container in same pod -**
              as a example two member of a room do not need a phone to talk to each other, they can talk with each other directly. just like this
two container of a same pod can communicate with each other directly by localhost. to test that, we will create two container one has ubuntu image and other has 
httpd image in a same pod. then we will go to ubuntu wala container and ping localhost:80, then it will be shown that It works. that means the communication is 
successfull.
```
#vi pod01.yml
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
    - name: c01
      image: httpd
      ports:
       - containerPort: 80
```
```

#kubectl apply -f pod01.yml
#kubectl exec testpod  -it -c c00 -- /bin/bash
      inside the container c00
	  # apt update && apt install curl
	  # curl localhost:80
	    op- it works
```	

**communication between two different pod in same node (each pod with one container) -**
                                                                                  - pod to pod communication in same node can be done with pod ip. but by
default we cant communicate with pod from outside of node.( if we want to access it from cluster or another pod then we have to use cluster ip service)
																				   
```
#vi pod02.yml
```
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod2
spec:
  containers:
    - name: c01
      image: httpd
      ports:
       - containerPort: 80
```
```

#kubectl apply -f pod01.yml
```


```
#vi pod03.yml
```
```
kind: Pod
apiVersion: v1
metadata:
  name: testpod3
spec:
  containers:
    - name: c01
      image: nginx
      ports:
       - containerPort: 80
```
```

#kubectl apply -f pod03.yml

#kubectl get pod -o wide ( to check ip of pods)

#curl <ip of pod 02>:80
  successfull
 #curl <ip of pod 03>:80
  successfull 
```  
  
  
