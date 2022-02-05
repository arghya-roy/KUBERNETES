---
NAMESPACES
---
we name comapre namespace as different branch in git, we can also comapre it in a another 
way like if house is a cluster then room is a namespace.
**to see all namespaces-**
```
kubectl get namespaces
```
**to see pods in a specific namespace-**
```
kubectl get pods -n namespace_name
```
**to see objects in a specific namespace-**
```
kubectl get object_name -n namespace_name
```
**to make a namespace named "dev" -**
```
apiVersion: v1
kind: Namespace
metadata:
   name: dev
   labels:
     name: dev
```
**create a pod and deploy it to "dev" namespace-**
```
vi pod.yml
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
      command: ["/bin/bash", "-c", "while true; do echo Technical Guftgu; sleep 5 ; done"]
  restartPolicy: Never   
```
```
kubectl apply -f pod.yml -n dev
```
**by default all things are happening in "default" but if we want to change the default namespace from "default" to "dev" -** 
```
$ kubectl config set-context $(kubectl config current-context) --namespace=dev
```
after running the above command all objects are going to deploy in dev namespaces without adding -n dev.
**to check that which namespace is set as default -**
```
$ kubectl config view | grep namespace:
```
