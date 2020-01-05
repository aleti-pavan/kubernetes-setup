# Useful Kubectl Commmands.

### Encode and decode

```

pavan$ echo "vania" | base64
dmFuaWEK

pavan$ echo "dmFuaWEK" | base64 --decode
vania
pavan$


```


### Create a Generic Secret with Dryrun
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

### Create a pod/deployment with imperative command

```

pavan$ kubectl run nginx --image=aletipavan/nginx-docker-example

kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/nginx created


pavan$ kubectl describe deploy nginx
Name:                   nginx
Namespace:              default


pavan$ kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5fb5f8c969-qlfsx   1/1     Running   0          33m


pavan$ kubectl describe po nginx-5fb5f8c969-qlfsx
Name:           nginx-5fb5f8c969-qlfsx
Namespace:      default
Priority:       0
Node:           minikube/192.168.64.4


```
### Get a Shell to a running container

```    
pavan$ kubectl exec -it nginx-5fb5f8c969-qlfsx -- /bin/sh           #bash isn't available for the container
/ # hostname
nginx-5fb5f8c969-qlfsx
/ # exit
pavan$


```

### Create Service for deployment - imperative commands

```

kubectl expose deployment/nginx
