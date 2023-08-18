# Minikube

It's one node cluster where both master processes and worker processes run in the same node, and this node will hve a docker container run time pre-installed. So `mnikube` will create a virtual box in the machine and it will run the node in the virtual box.

# Kubectl

It's a command line tool to interact with the cluster, creating pods and other components of k8s in the nodes.

Master processes -> Api server
You can talk to the api server vis as UI dashboard, api or cli (kubectl)

# Deployment

### Template

It's a blueprint for the pods within a deployment configuration, it has its own metadata and spec just like deployment configuration

```
template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

This configures the deployment to create pods with a Nginx image.

### Labels

### MatchLabels
