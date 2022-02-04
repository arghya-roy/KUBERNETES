**deployment-
           deployment is just like version control system. suppose now we are using ubuntu image in a pod an runiing do echo technical guftgu command in it.
after that if we change the command from technical guftgu to arghya roy, then it make a new version.
                                   1) we can recover the previous version where technical guftgu running.
		                   2) while we using new version then old version of replica set is stopped but not deleted, but pod under replica set will be deleted.
				   3) when we recover old version ( roll back )then replicaset creates pod with new ip.
				   4) suppose we were running 4 replicas in old versions and we are using 5 replicas in new version.
				      then when we go from new version to old version then it will have 5 replicas not 4.
				   5) deployment > replica set > pod > containers
                                   6) suppose we have v1 v2 v3 v4 then we can also roll back from v4 to v1.
				   7) replica set in mandotory ti have in case of using deployment.
				   8) rollback-
				            we are in v4 and we want to go to v1 it is called roll back.
				      rollout- 
					        we are in v2 and we want to go to v3 then it is called roll out or update.
				   9) suppose we are running v2 in node1. now if we want to roll back to v1 then pod of v1 can be created in nod1 or any other node.
				   10) when we do rollout or rollback then deployment object orders to replicaset to create pod for that versions as per decleared replic number.
                                   11) we can pause deployment.
				   12) we can delete versions.
				   
				   
    rollback to a specific previous version-
	                                kubectl rollout undo deploy/name --to-revision=2   # 2 or any specific version
	to go just previous version - 
									kubectl rollout undo deploy/name
	name of replica set-
	                 [deployment_name][random string]
	to check how deployment created rs and pods-
	                 kubectl describe deploy mydeployments
	to check what is running inside containers-
	                 kubectl log -f podname
	to scale up or scale down -
	                 kubectl scale --replicas=1 deploy name
	to check status of roll back -
	                 kubectl rollout status deployment name
	to check history or how many version you have of rollback -
	                 kubectl rollout history deployment name
					 
					 
reasons for deployment failed -
1) insufficient quota -
         dont have required resourses in node.
2) rediness probe failure
3) image pull error
4) insufficient permission
5) limit ranges
6) application rumtime misconfiguration
	

vi deploy1.yml

kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5; done"]
		  
		  
# kubectl apply -f deploy1.yml
# kubectl get deploy
# kubectl log -f podname

now we are changing the command 

vi deploy1.yml

kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo love-you; sleep 5; done"]
		  
		  
# kubectl apply -f deploy1.yml
# kubectl get rs
      o/p- it will show that we have two replicaset under this development. but status of previous one is zero, that means the previous one is stopped.
# kubectl log -f podname

# kubectl rollout undo deploy/name
# kubectl log -f podname




