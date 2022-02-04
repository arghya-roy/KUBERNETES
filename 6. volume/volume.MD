
**volume-**
```
 it is used to share volumes between two or multiple container in a same pod and share volume between pod and host machine.
 
 there are mainly two types of volumes in kubernetes
    1) emptyDir
		2) hostPath
```		
**emptyDir:**
```
         suppose we have a pod and in this pod we have two container. if we want to share a volume between those two
container then we can use emptyDir.
here we have to define a volume which will be located in othat pod. and thhose two container shares that volume. if one container
deleted then the new container will automatically share the volume.
we have to remember that the volume is located in pod, it pod gets deleted then that volume will also
be deleted.
```
```
#vi myvolemptydir.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: myvolemptydir
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:                                    # Mount definition inside the container
      - name: xchange               # same name have to give for c1 and c2
        mountPath: "/tmp/xchange"   # a folder called xchange will be created in c1.      
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
      - name: xchange               # same name have to give for c1 and c2
        mountPath: "/tmp/data"      # a folder called data will be created in c2.
  volumes:                          # here we are defining the volume                        
  - name: xchange                   #  the name should be same as volume name of c1 and c2.
    emptyDir: {}                    # delearing the type of volume.


```
```
#kubectl apply -f myvolemptydir.yml
```
if we create in file in c1 in /tmp/xchange folder then the same file will be seen from c2 of /tmp/data folder.
---
**hostPath-**
```
         there was mainly two disadvantage in emptyDir.
		     1) we cannot share the volume between two pods
			 2) if the pod gets deleted then we cant access the volume also.
to overcome these disadvantage we can use hostPath type of volume where we can map the volume of container with host machine.
so in this cases if pods get deleted also then we can access the volume from host machine. and also we can 
share the volume with two or multiple pods.
```
```
# vi myvolhostpath.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: myvolhostpath
spec:
  containers:
  - image: centos
    name: testc
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:
    - mountPath: /tmp/hostpath       # volume path of container
      name: testvolume               # the name should be same as name of host volume name.
  volumes:                           # here we are declearing the volume
  - name: testvolume
    hostPath:                        # the name should be same as name of container volume name.
      path: /tmp/data                #volume path of host machine
```
```
# kubectl apply -f myvolhostpath.yml
```	  
supose we are using ec2 then if we create a file in /tmp/data in host machine then we can see the same file from
/tmp/hostpath of container.
