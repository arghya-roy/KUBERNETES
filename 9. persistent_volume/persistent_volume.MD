**PERSISTENT VOLUME-**
```
                It is a cluster based resourse, means all nodes inside a cluster can access the persistent volume.
in previous video we have learnt about hostpath. incase of hostpath it is located on a single node but after destroying the pod, the new can be 
created on a different node. in that the new pod cannot access its previous hostpath location.
as a solution of this problem we use persistent volume. 
in case of persistent volume, it is connected to all nodes, so if pod creates on different node also then the pod can access its previous storage.

3 steps to do persistent volume -
    1) create a EBS
	 2) create persistent volume- means if we have 20 gb EBS then we can make 5 persistent volume with 4 gb storage and also other ratio.
	 3) persistent volume claim - suppose we need a 1gb storage for pod. then we claim 1 gb storage. after that 1 gb storage can be assign 
	                              from any of those 5 persistent volume. ( suppose we have 3 persistent volume 3gb, 4gb, 6gb and we have done
								                a 5gb volume claim. then it will be assigned from 6gb persistent volume)
	 4)attaching the persistentVolumeClaim to pods.

There are some restrictions-
   1) pods are must be running on ec2.
	 2) node and EBS should be in same region and same availability zone.
	 EBS only supports a single ec2 instance mounting a volume at a time.
```	 
---
1) create a EBS and copy its ID
---
2) create a 1GB persistent volume from that EBS
---
```
#vi persistentvolume.yml
```
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: myebsvol
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  awsElasticBlockStore:
    volumeID:           # paste EBS ID here
    fsType: ext4
```
```
#kubectl apply -f persistentvolume.yml
#kubectl get pv 
```
---
3) doing 1gb persistentvolume claim from previously created persistentvolume. in case there is multiple persistent volume
   then persistentVolumeClaim can be happened from any of the persistent volume.
---
```
vi persistentVolumeClaim.yml
```
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myebsvolclaim   / same name will be given while attaching it to pod.
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
```
# kubectl apply -f persistentVolumeClaim.yml
# kubectl get pvc
```
---
4) attaching that persistentVolumeClaim to pod.
```
#vi pod.yml
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pvdeploy
spec:
  replicas: 1
  selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     app: mypv
  template:
    metadata:
      labels:
        app: mypv
    spec:
      containers:
      - name: shell
        image: centos
        command: ["bin/bash", "-c", "sleep 10000"]
        volumeMounts:
        - name: mypd
          mountPath: "/tmp/persistent"      // this is the folder inside the container which is connected to the myebsvolclaim 
      volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: myebsvolclaim    // same name which is given at persistent volume claim
```
```
			
# kubectl apply -f pod.yml
```

**to check the process -**
```
#kubectl ecec pod_name -it -- /bin/bash
#cd /tmp/persistent
#touch file1 
#kubectl delete pod podname
```

**now by replica set it will create new pod-**
```
# kubectl ecec pod_name -it -- /bin/bash
# cd /tmp/persistent
# ls
```
