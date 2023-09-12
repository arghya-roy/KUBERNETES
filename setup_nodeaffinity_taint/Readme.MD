## to add label to node -
- `kubectl label node node01 color=blue`
- `kubectl label node node02 color=green`  #optional
## to add taint to node -
- `kubectl taint nodes node01 application=example:NoSchedule`
- `kubectl taint nodes node02 application=example:NoSchedule` 

## create pod with node affinity and tolerance -
```
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
  - key: application
    value: example
    effect: NoSchedule
    operator: Equal
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: color
            operator: In
            values:
            - blue
            - green # optional
```
