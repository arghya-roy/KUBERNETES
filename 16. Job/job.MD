
#### Job-
by using kind: job, it also creates pod. but this pod runs only one time.after completeing the work it
termenetes automatically. In the below example it runs echo Technical-Guftgu for 5 secs, after that
pod will be terminated.
``
vi job1.yml
```
```
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  template:
    metadata:
      name: testjob
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
      restartPolicy: Never
```
**parallelism -**  
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  parallelism: 5                           # Runs for pods in parallel
  activeDeadlineSeconds: 10  # Timesout after 30 sec
  template:
    metadata:
      name: testjob
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 20"]
      restartPolicy: Never
-------------
apiVersion: batch/v1beta1
kind: CronJob
metadata:
 name: bhupi
spec:
 schedule: "* * * * *"
 jobTemplate:
   spec:
     template:
       spec:
         containers:
         - image: ubuntu
           name: bhupi
           command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
         restartPolicy: Never
=================Init Containers==
apiVersion: v1
kind: Pod
metadata:
  name: initcontainer
spec:
  initContainers:
  - name: c1
    image: centos
    command: ["/bin/sh", "-c", "echo LIKE AND SUBSCRIBE TECHNICAL GUFTGU > /tmp/xchange/testfile; sleep 30"]
    volumeMounts:        
      - name: xchange
        mountPath: "/tmp/xchange"  
  containers:
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo `cat /tmp/data/testfile`; sleep 5; done"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:                            
  - name: xchange
    emptyDir: {}
=================
