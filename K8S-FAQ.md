What is difference between Replication Controller, Replica Sets and Deployment

Replication Controller:
Replication controller makes sure the desired number of pods are running all the time. It the original form of k8s.

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-pod
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

