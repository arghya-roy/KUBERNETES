**Configmap -**
configmap can be defined in two types. main difference between congigmap and secret is that anyone can read configmap content but only 
container can read secret files. as a example when we need to pass database url then we use configmap and when we pass password then we use secret.
#### 1. by volume - 
at first we create a file with content and then we declere that file as configmap. then this file works as a virtual volume.
after that when any container has to access the content of the file, then we mounth the volume.
 ```
 vi file_name.conf
 write content
 ```
####   **to decleare file a configmap-**
   ```
   kubectl create configmap name1 --from-file=file_name.conf
   ```
   **add this file as a configmap to pod-**
```   
   vi pod.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: myvolconfig
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    volumeMounts:
      - name: testconfigmap
        mountPath: "/tmp/config"   # the config files will be mounted as ReadOnly by default here
  volumes:
  - name: testconfigmap   #any name
    configMap:
       name: name1   # this should match the config map name created in the first step
       items:
       - key: file_name.conf  #file name
         path: file_name.conf  #file path
```

##### 2. by ENV -
         when we use configmap as a ENV then in the container the content of file is stored as a value and the key we have to define while creating container.
 ```
 vi file_name.conf
 write content
 ```
**to decleare file a configmap-**
```
   kubectl create configmap name2 --from-file=file_name.conf
```

```
vi pod2.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: myenvconfig
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    env:
    - name: mykey         # this is the key in which value of the key is stored
      valueFrom:
        configMapKeyRef:
          name: name2      # name of the config created
          key: file_name.conf
```
