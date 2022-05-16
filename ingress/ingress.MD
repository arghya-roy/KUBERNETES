### deploy nginx controller -
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
name: nginx-ingress-controller
spec:
replicas: 1
selector:
matchLabels:
name: nginx-ingress
template:
metadata:
labels:
name: nginx-ingress
spec:
containers:
- name: nginx-ingress-controller
image:quay.io/kubernetes-ingresscontroller/nginx-ingress-controller:0.21.0
args:
- /nginx-ingress-controller
- --configmap=$(POD_NAMESPACE)/nginx-configuration
env:
- name: POD_NAME
valueFrom:
fieldRef:
fieldPath: metadata.name
- name: POD_NAMESPACE
valueFrom:
fieldRef:
fieldPath: metadata.namespace
ports:
- name: http
containerPort: 80
- name: https
containerPort: 443
```
### Create config map
```
kind: ConfigMap
apiVersion: v1
metadata:
name: nginx-configuration
```
### create service account with role and rolebinding -
```
apiVersion: v1
kind: ServiceAccount
metadata:
name: nginx-ingress-serviceaccount
```
### expose nginx controller -
```
apiVersion: v1
kind: Service
metadata:
name: nginx-ingress
spec:
type: NodePort
ports:
- port: 80
targetPort: 80
protocol: TCP
name: http
- port: 443
targetPort: 443
protocol: TCP
name: https
selector:
name: nginx-ingress
```
### Create resourses with path -
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: ingress-wear-watch
spec:
rules:
- http:
paths:
- path: /wear
backend:
serviceName: wear-service
servicePort: 80
- path: /watch
backend:
serviceName: watch-service
servicePort: 80
```
### create resouses with subdomain -
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: ingress-wear-watch
spec:
rules:
- host: wear.my-online-store.com
http:
paths:
- backend:
serviceName: wear-service
servicePort: 80
- host: watch.my-online-store.com
http:
paths:
- backend:
serviceName: watch-service
servicePort: 80
```
