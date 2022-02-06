horizontal pod autoscaling means pod will increaes automatically when cpu uses increases. to do that we have to download metrics-
```
$ wget -O metricserver.yml https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
```
vi metricserver.yml
```

after that search **kind: deployment** then under **args:** type 3rd line - **- --kubelet-insecure-tls**
#### then that is looks like - 
```
spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=4443
        - --kubelet-insecure-tls
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        image: k8s.gcr.io/metrics-server/metrics-server:v0.6.0
 ```
 ```
 kubectl apply -f metricserver.yml
 ```
--------------
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeploy
spec:
   replicas: 1
   selector:
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod8
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
          resources:
            limits:
              cpu: 500m
            requests:
              cpu: 200m
---------------------
$ kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10
