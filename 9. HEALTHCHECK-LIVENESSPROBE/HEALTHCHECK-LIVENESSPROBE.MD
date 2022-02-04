
**HEALTHCHECK/LIVENESSPROBE -** 
```
resposibility of replica set is just maintain current pod status equal to desired pod status.
but suppose we have deployed something inside a container then how can i know that is it running or not?
to know that we have to check HEALTHCHECK/LIVENESSPROBE.

it runs a specific command related to the application of  container after a specific interverl and check is it running?
if the output is zero, then it is running.
if the output is any number other than zero, then it is not in right condition, then it deletes the container and install a new.
```
```
vi pod1.yml
```
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: mylivenessprobe
spec:
  containers:
  - name: liveness
    image: ubuntu
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 1000
    livenessProbe:                                          
      exec:
        command:                                         
        - cat                
        - /tmp/healthy
      initialDelaySeconds: 5     // after creating the container it waits for 5 secs and check it      
      periodSeconds: 6 // after previous check at waits 6 secs and check again	  
      timeoutSeconds: 30     // if within 30 sec there is no reply, then it is unhealthy and delete this container and create a new.                             
```



			
			
			


