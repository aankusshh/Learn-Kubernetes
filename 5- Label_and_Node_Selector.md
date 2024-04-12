## Labels and selectors

- Labels are the mechanism you use to organise Kubernetes objects ( object can be anything pod, Deployment and etc)
- A label is a key-value pair without any predefined meaning that can be attached to the objects Eg:- Name: Ankush
       Class: Pod
- Labels are similar to tags in AWS or git where you use a name to quick refrence
- So you are free to choose labels as you need it to refer an environment which is used for dev or testing or production, refer a product group like department A, department B
- Multiple labels can be added to a single object

### label.yml

```bash
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

```bash
kubectl apply -f pod.yml
kubectl get pods --show-labels
```

## Manifest file

Now if we want to add a label to an existing pod:

```bash
kubectl label pods <pod_name> environment=production
kubectl get pods --show-label
```

Now list pods matching a label

```bash
kubectl gets pods -l environment=production
```

Now give a list, where “production “ label is not present

```bash
kubectl gets pod -l env != production
```

Now if you want to delete pod using labels

```bash
kubectl delete pod -l env != development
kubectl gets pods
```

## Label Selector

- Unlike name/Uid’s , labels do not provide uniqueness, as in general, we can expect many objects to carry the same label
- Once labels are attached to an object, we could need filters to narrow down and these are called as Label selectors
- The api currently supports two types of selectors
    - Equality base and
    - Set based
- A label selector can be made of multiple requirement which are comma- separated

### Equality based (=, !=, ==)

*Equality-* or *inequality-based* requirements allow filtering by label keys and values.
 Three kinds of operators are admitted `=`,`==`,`!=`

## set based (in, notin, exists)

- **env in (production, dev)** - It will select those pods which are labeled with either production or dev.
- **env notin (team1, team2 )** - It will select all those which don’t have labels called team1 and team2.

```bash
# Set based
kubectl gets pods -l 'env in (development, testing)'
kubectl gets pods -l 'env notin (development, testing)'

# Equality based
kubectl get pods -l class=pods, myname=ankush
```

## Node selector

- One use case for selecting labels is to constrain the set of nodes onto which s pod can schedule i.e. you can tell a pod to only be able to run on a particular nodes
- Generally such constraints are necessary as the scheduler will automatically do a resonable placement, but on certain circumstances we might need it
- We can use labels to tag nodes
- Once the nodes are tagged, now you can use the label selector to specify on which node the pod should run.
- First we give label to the node
- Then use node selector in the pod configuration

### Labelling of the node

```bash
kubectl get node
kubectl label node <nodename> myname=ankush

# Now we will configure nodeSelector.yml file
kind: pod
apiVersion: V1
metadata: 
	name: nodelabels
	labels: 
		env: development
spec:
	containers:
	- name:C00
		image:ubuntu
		command: ["/bin/bash". "-c", "while true; do echoHello; sleep 10; done"]
	nodeSelector:
		myname: ankush
```
