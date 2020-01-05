# Useful Kubectl Commmands.


### Create a Generic Secret
```
pavan$ kubectl create secret generic s1 --from-literal=name=vania --dry-run
secret/s1 created (dry run)

pavan$ kubectl create secret generic s1 --from-literal=name=vania --dry-run -o yaml
apiVersion: v1
data:
  name: dmFuaWE=
kind: Secret
metadata:
  creationTimestamp: null
  name: s1

pavan$ echo "dmFuaWE=" | base64 --decode


```
