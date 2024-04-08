# Kubernetes Objects

- Every work in manifest is treated as object
- Kubernetes uses objects to represent the state of your cluster.
- What containerized applications are running and on which node.
- The policies around how those applications behave such as restart policies upgrades and fault tolerance.
- Once you create the object the Kubernetes system will constantly work to ensure that the object exists and maintains the cluster's desired state.
- Every Kubernetes object includes two nested fields that govern the object configure the object specifications (desired state) and object status (actual state).
- This specification which we provide, describes your desired state for the object and the characteristics that you want the object to have.
- This status described the actual state of the object.
- All objects are identified by a unique name or UID.

## Basic Objects in Kubernetes

| Pod | Secrets |
| Service | ConfigMaps |
| Volume | Deployment |
| NameSpace | Jobs |
| ReplicaSets | DaemonSets |
- It is represented in json or yml file.
- You create these and then push them to the kubernetes API with kubectl

### Relationship between these object:

- Pod manages containers.
- Replica set manages pod.
- Services exposed pod processes to the outside word.
- Configmap and secrets helps you to configure pods.

## State of the Objects

- Repicas
- Image
- Name
- Port
- Volume
- Startup (cmd runs when get start)
- Detached

## Fundamental of Pods

- When a pod gets created, it is scheduled to run on a node in your cluster.
- The pod remains on that node until the process is terminated, the pod object is deleted, the pod is evicted for lack of resources or the node fails.
- If a pod is scheduled to a node that fails or if the scheduling operation itself fails, the pod is deleted.
- If a node dies , the pods scheduled to that node are scheduled for deletion after a **timeout period**
- A given pod (UID) is not “rescheduled” to a new node, instead it will be replaced by a identical pod, with even the same name if desired, but with a new UID.
- Volume in a pod will exists as long as that pod (with that UID) exist, If that pod is delete for any reason, volume is also destroyed and created as new on new pod.
- A controller can create and manage multiple pods, handling replication, rollout and providing self-healing capabilities.

## Kubernetes Configuration

- All-in-one → single node installation (Minikube)
- Single-node , single master or single master, multiple nodes.
- Single node, multiple master or multiple master, multiple nodes (multiple masters provide high availability).

# Demo Manifest File

```bash
apiVersion: apps/v1
kind: Deployment #object -> Deployment
metadata:
  name: nginx-deployment   
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
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

## Components of yaml manifest (Identation matters in yaml file)

### 1. Annotation :

When we want to add extra information about the .yml file (like date, descriptions)

Example → File name: pod1.yml

```bash
"metadata": {
  "annotations": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

```bash
kubectl apply -f pod1.yml
#apply save aal the changes in the menifest
# -f force apply 
```

## 2. Environment Variables

When we want to pre-define same variables in pod

Example: File name → testpod.yml

```bash
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
```

```bash
kubectl apply -f testfile.yml
kubectl get pods
kubectl exec envar-demo -- printenv
```

### Output:

```
NODE_VERSION=4.4.2
EXAMPLE_SERVICE_PORT_8080_TCP_ADDR=10.3.245.237
HOSTNAME=envar-demo
...
DEMO_GREETING=Hello from the environment
DEMO_FAREWELL=Such a sweet sorrow
```

## Pod with Port

Example: File name → demoport.yml

```bash
spec:
	containers:
	- name: C00
		image: httpd #apache Server
		ports:
		-  containerPort: 80
```

```bash
Kubectl apply -f demoport.yml
kubectl get pods
kubectl get pods -o wide #gives you all the info about your pod 
#and you will get pod IP and instance IP
curl <IP>:80
```

## Output:

```bash
It works
```
