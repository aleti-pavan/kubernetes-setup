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

### Create deployment with yaml file

```

pavan$ cat nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      run: nginx
  replicas: 2
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - name: nginx
        image: aletipavan/nginx-docker-example
        ports:
        - containerPort: 80

pavan$ kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx created

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

kubectl expose deployment nginx --port=8000 --type=NodePort

```



## Kubectl Deployment Rollout


We tried creating deployment, replicaset, pods and service. When we are trying to access `ClusterIP` type of service from the `Pod` itself the image we have used doesn't have `curl` command. So we are changing the image we have used for the deployment.

__Change__ : `aletipavan/nginx-docker-example` ---->  `nginx:1.9.1`

```

pavan$ kubectl expose deployment nginx --port=8000 --target-port=80
service/nginx exposed
pavan$ kubectl get svc
NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.96.0.1     <none>        443/TCP    5h15m
nginx        ClusterIP   10.97.62.56   <none>        8000/TCP   5s
pavan$ kubectl get all
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-79fdd7bfbd-wk7n9   1/1     Running   0          71m
pod/nginx-79fdd7bfbd-zcgk2   1/1     Running   0          71m


NAME                 TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
service/kubernetes   ClusterIP   10.96.0.1     <none>        443/TCP    5h15m
service/nginx        ClusterIP   10.97.62.56   <none>        8000/TCP   14s


NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx   2/2     2            2           71m

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-79fdd7bfbd   2         2         2       71m




pavan$ kubectl exec -it nginx-79fdd7bfbd-wk7n9 curl 10.97.62.56:8000
OCI runtime exec failed: exec failed: container_linux.go:345: starting container process caused "exec: \"curl\": executable file not found in $PATH": unknown
command terminated with exit code 126


curl command doesn't exist in the image we have used, hence we are changing the image. This process is as same as we do app version change from one version to other version of the applicaiton.


pavan$ kubectl set image deployment nginx nginx=nginx:nginx:1.13.12
deployment.apps/nginx image updated
pavan$ kubectl rollout status deployment nginx
Waiting for deployment "nginx" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "nginx" rollout to finish: 1 old replicas are pending termination...
deployment "nginx" successfully rolled out
pavan$

pavan$ kubectl describe node
Name:               minikube
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Sun, 05 Jan 2020 20:20:20 +0000   Sun, 05 Jan 2020 14:14:35 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  Ready            True    Sun, 05 Jan 2020 20:20:20 +0000   Sun, 05 Jan 2020 14:14:35 +0000   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.64.4
  Hostname:    minikube


kubectl delete deployment nginx


kubectl delete svc nginx


kubectl apply -f manifest-files/nginx-deployment.yaml


Using the kubernetes cluster IP i.e minikube's IP in our case. we can access the pod.

pavan$ curl 192.168.64.4:30555
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
pavan$




```
