Install Mario Pod and Generate Stack Trace
Let’s Install another pod to the cluster! 
kubectl create ns mario

First let’s vi a file to copy our deployment and service yaml into:

`vi <yourNameHere>-mario-deploy.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: <yourNameHere>-mario
  name: <yourNameHere>-mario
spec:
  replicas: 1
  selector:
    matchLabels:
      app: <yourNameHere>-mario
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: <yourNameHere>-mario
    spec:
      containers:
      - image: pengbai/docker-supermario
        name: docker-supermario
        ports:
        - containerPort: 8080  
        resources: {}
status: {}
---
apiVersion: v1
kind: Service
metadata:
  name: <yourNameHere>-mario
spec:
  type: NodePort
  selector:
    app: <yourNameHere>-mario
  ports:
    - port: 8080 
      targetPort: 8080
      protocol: TCP
      nodePort: 30080
```

Install The Mario deployment and service

```
kubectl -n mario apply -f <yourNameHere>-mario-deploy.yaml
```


Use the kubectl watch command to make sure your pod is running

```
kubectl -n mario get pods -w
```

Now let's look at the logs our pod is emitting on the command line (be sure to use your pod’s name from the “get pods” command!)

```
kubectl -n mario logs -f <yourNameHere>-mario-<xxx>
```
Let's look at how they look in Splunk!

```
index="otel_events" sourcetype="kube:container:docker-supermario" | reverse
```


Navigate to the following address to access your pod from the browser:

`http://<yourHostname>.foo.io:30080/`

Take a minute and play around a bit! Use your arrow keys to move mario. Use the “s” key to select a level or to jump. 

Now, let's cause an error in our pod logs. The pod doesn't serve https, so try connecting to the same url with https. 

`https://<yourHostname>.foo.io:30080/`

What happens? 

Check your pod logs from the command line

```
kubectl -n mario logs -f <yourNameHere>-mario-<xxx>
```

Proceed to [Module 3](https://github.com/matthewmodestino/otel-quickstart/blob/main/kubernetes/3-module-three.md)

