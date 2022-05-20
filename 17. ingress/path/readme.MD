### deploy ingress controller -
```
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update
helm install ingress-nginx nginx-stable/nginx-ingress
helm install ingress-nginx nginx-stable/nginx-ingress --set rbac.create=true
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/cloud/deploy.yaml
```
### to check -
```
kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx
kubectl get services ingress-nginx-controller --namespace=ingress-nginx
```

### Deployment-service 1 -
```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
---
kind: Service                             # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80                             # Containers port exposed
      targetPort: 80                 # Pods port
  selector:
    name: deployment                    # Apply this service to any pods which has the specific label
```
### deployment-service 2 -
```
kind: Deployment
apiVersion: apps/v1
metadata:
   name: drupal
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
    matchLabels:
     name: drupal
   template:
     metadata:
       name: drupal
       labels:
         name: drupal
     spec:
      containers:
        - name: c00
          image: nginx
          ports:
          - containerPort: 80
---
kind: Service                             # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice2
spec:
  ports:
    - port: 80                               # Containers port exposed
      targetPort: 80                   # Pods port
  selector:
    name: drupal                    # Apply this service to any pods which has the specific label
```
### ingress resources -
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-fanout-example
spec:
  rules:
  - host: fooo.gyanhut.com
    http:
      paths:
      - path: /foo
        pathType: Prefix
        backend:
          service:
            name: demoservice
            port:
              number: 80
      - path: /bar
        pathType: Prefix
        backend:
          service:
            name: demoservice2
            port:
              number: 80
  ```
### kubectl get ingress
map host with same ip
