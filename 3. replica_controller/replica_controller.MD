- when we use kind: pod then if this pod is deleted then it can never by created automatically. to create it automatically we will leran it here
by using kind:replicationController.

- we can say that pods are controlled by replicationController.

- containers < pods < replicationController

- here if we spefify replicas: 2 then desired state of that pod is 2. if we delete one pod manually then derired state and current state does not met. so 
as a result of this one pod will be created to match the desired state with current state. the working of new pod and previous pod are exactly same but the 
ip is different.

- if the replicas: 1 then if we delete that pod then 1 pod will be automatically created to match current state and desired state.
    - kind should be ReplicationController
    - under spec of replicationController we need to tell a selector.
    - same selector will be given under pod spec as a label.
    - then selector of replicationController select the label from pod spec and manage that pod by that replication Controller.
    - if replicas:2 then those two pods are identicals to each other.

**replicationController works on equality based selector. there is a another replication system named replica set which supports both equality based
and set based selector. we will read about replica set later.**






```
#vi rcpod.yml
```
```
kind: ReplicationController               
apiVersion: v1
metadata:
  name: myreplica            #name of replicationController
spec:
  replicas: 2                #number of pods to replicate.
  selector: 
   matchLabels:
    myname: Bhupinder        # this selector should be same as labels of pod, selector of RC matches the labels of pods to replicate or control this.
  template:                  # this template = kind: Pod and apiVersion: v1
    metadata:
      name: testpod6         # name of pod
      labels:            
        myname: Bhupinder    # label of pod
    spec:
     containers:             # details of containers under pod
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
		 
```
```
#kubectl apply -f rcpod.yml
```
**to see replicationController**
```
#kubectl get rc
```
**to see pods**
```
# kubectl get pods
```
**to see details of a rc**
```
#kubectl describe rc myreplica
```
**to delete rc**
```
#kubectl delete rc NAME
```
**To delete a replication controller without deleting its pods, use kubectl delete and specify --cascade=false:**
```
#kubectl delete rc NAME --cascade=false
```
**to see that what is happening in container. if pod has multiple container then we have to specify container name.**
```
# kubectl logs -f pod_name
```
**if have multiple container**
```
# kubectl logs -f pod_name -c container_name
```
**to increase or decrease replicas no in a replicationController, it matches pod with their labels and control replicas number.**
```
# kubectl scale --replicas=x rc -l myname=bhupinder
                    or
# kubectl scale --replicas=x rc/myreplica
```
