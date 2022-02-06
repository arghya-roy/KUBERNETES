**secrets -**
secret can be defined in two types. main difference between congigmap and secret is that anyone can read configmap content but only 
container can read secret files. as a example when we need to pass database url then we use configmap and when we pass password then we use secret.
#### 1. by volume - 
at first we create a file with content and then we declere that file as configmap. then this file works as a virtual volume.
after that when any container has to access the content of the file, then we mounth the volume.
 ```
 vi username.txt
 write content
 ```
####   **to decleare file a secret-**
   ```
kubectl create secret generic mysecret --from-file=username.txt
```
   **add this file as a configmap to pod-**
```   
   vi pod.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: myvolsecret
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-guftgu; sleep 5 ; done"]
    volumeMounts:
      - name: testsecret
        mountPath: "/tmp/mysecrets"   # the secret files will be mounted as ReadOnly by default here
  volumes:
  - name: testsecret
    secret:
       secretName: mysecret  
```

##### 2. by ENV -
        
