# What is difference between Replication Controller, Replica Sets and Deployment

# Replication Controller:
Replication controller makes sure the desired number of pods are running all the time. It the original form of k8s.

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-rc
spec:
  replicas: 3
  selector:
    app: my-pod
  template:
    metadata:
      name: my-pod
      labels:
        app: my-pod
    spec:
      containers:
      - name: my-pod
        image: my-pod
        ports:
        - containerPort: 80
```

As you delete replication controller it will delete all pods too.


# Replica Set
Its very similar to replication controller just that it has more options for selector like matchlabels and matchexpressions.

```yaml
apiVersion: extensions/v1beta1
 kind: ReplicaSet
 metadata:
   name: my-rs
 spec:
   replicas: 3
   selector:
     matchLabels:
       app: my-pod
   template:
     metadata:
       labels:
         app: my-pod
         environment: dev
     spec:
       containers:
       - name: my-pod
         image: my-pod
         ports:
         - containerPort: 80
```

# Deployment

Deployment is upgraded version if RC and RS. It provide same features as RS and can rollout changes and roll them back.

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 5
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app
        ports:
        - containerPort: 80
```

Same as RC, RS it will delete all pods if deployment is deleted

Following are come cmd examples:

Create a deployment:
```shell
kubectl run my-nginx --image=nginx --replicas=2 --port=80
```

Creates a service of type node port:
```shell
kubectl expose deployment my-rc --port=80 --target-port=80 --type=NodePort
```

Scale up by:
```shell
kubectl scale --replicas=7 deployment/my-rc
```

Add/replace label:
```shell
kubectl label pods my-app-1234-xyz experimental=true
kubectl label pods my-app-1234-xyz app=my-app-say --overwrite
```


