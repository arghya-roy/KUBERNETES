**replicationController works on equality based selector. there is a another replication system named replica set which supports both equality based
and set based selector. we will read about replica set here.**
--
**replicationController was in previous version so for that case the appversion was v1. but replication set is at new version so for this case
apiVersion is apps/v1.**
--
```
#vi repod.yml
```
```

kind: ReplicaSet                                    
apiVersion: apps/v1                            
metadata:
  name: myrs
spec:
  replicas: 2  
  selector:                  
    matchExpressions: # any of these must match the labels, if any of these are mathing with label of pod then it controls the pods.
      - {key: myname, operator: In, values: [Bhupinder, Bupinder, Bhopendra]} 
      - {key: env, operator: NotIn, values: [production]}
  template:        # this template = kind: Pod and apiVersion: v1      
    metadata:
      name: testpod7
      labels:              
        myname: Bhupinder
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
		 
```
```
#kubectl apply -f rspod.yml
```
**to see replication set**
```
#kubectl get rs
```
**to see pods**
```
# kubectl get pods
```
**to see details of a rs**
```
#kubectl describe rs myrs
```
**to delete rs**
```
#kubectl delete rs NAME
```
**To delete a replication set without deleting its pods, use kubectl delete and specify --cascade=false:
```
#kubectl delete rs NAME --cascade=false
```
**to see that what is happening in container. if pod has multiple container then we have to specify container name.**
```
# kubectl logs -f pod_name
```
**if have multiple container**
```
#kubectl logs -f pod_name -c container_name
```

**to increase or decrease replicas no in a replicationController, it matches replicationController with their selector and control replicas number.**
```
# kubectl scale --replicas=x rs -l myname=bhupinder
               or
# kubectl scale --replicas=x rs/myrs
```		 
		 
		 
		 
