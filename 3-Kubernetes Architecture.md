# Kubernetes Architecture

## Working with Kubernetes

- We create manifest file (either .yml or .json).
- Applies to cluster (to master) to bring into Desire state.
- Pods run on nodes which is controlled by the master.

## Roles of master node

- Kubernetes clusters contains containers running on bare metal (physical servers), VM instances, cloud instances all mix.
- Kubernetes designate one or more of these as master and all others as workers.
- The master is now going to run set of kubernetes processes.
- These processes will ensure smooth functionings of the cluster.
- These process are called control plane.
- Can be multimaster for high availability.
- Master runs control plane to run cluster smoothly.

# ARCHITECTURE
![Kubernetes Architecture](Kubernetes-cluster-architecture.ppm)


# Components of control plane (Master)

- Kube-api
- Etcd cluster
- Kube scheduler
- Controller manager

## Kube-api-Server

- For all communication in between the components.
- User or client use this to communicate with the master.
- Exposes to the outer word with the help of cli.
- This API-server internet directly with the user (that is we apply .yml or .json manifest to kube-api-server).
- This kube-api-server is meant to scale automatically as per the load.
- Kube-api-server is the front end of the control plane

## ETCD

- Stores matadata and status of cluster.
- etcd is consistent and high available Store (key-value store).
- Source of touch for cluster state (gives information about state of cluster or pods).
- **etcd has the following feature:**
    - **Fully replicated:** The entire state is always available on every note in the cluster. (becoz as soon as the pod goes down it will inform the api-server and new pod will be created).
    - Secure: Implements automatic TLS (transport layer security) with optional client certificate authentication.
    - Fast: Benchmarked at 10000 writes per second.

## Kube-scheduler

- When users make request for the creation and management of pods kube-scheduler is going to take action on these request.
- Handles pods creation and Management.
- Kube-scheduled matche/assign any node to create and run pods.
- A schedule watches for newly created pods that have no node assigned, for every pod that the scheduler discovered, the scheduler becomes responsible for finding the best node for that pod to run on.
- Scheduler get the information for Hardware configuration from configuration files and schedules the pods on nodes accordingly.

## Control manager

- Maintain actual state equals to desire state
- Make sure actually state of cluster matches the desired state.
- Two possible choices for control manager:
    - If kubernetes on cloud then it will be cloud-controller-manager.
    - If kubernetes on non cloud then it will be kube-control-manager.
- Components on master that run on controller:
    - **Node controller: F**or checking the cloud provider to determine if a node has been detected in the cloud after it stops responding.
    - **Route controller: R**esponsible for setting up network routes on your cloud.
    - **Server controller:** Responsible for load balancer on your cloud against services of type load balancer.
    - **Volume Controller:** For creating attaching and mounting volumes on interacting with the cloud provider to orchestrate volume.
    

# Worker node components

- Kubelet
- Container Engine
- Kube Proxy

## Kubelet

- Ensures that pod is always running.
- Agent running on nodes.
- Listen to cubin it is Master (eg - pod creation request).
- Use port 10255.
- Send success/failure report to master node (api-server).

## Container engines

- Actually runs the container.
- Work with kubelet.
- Pulling images (conatiner images).
- Start/stop container.
- Exposing containers on ports that are specified in manifest.

## Kube proxy

- Assign IP to each pod.
- It is required to assign IP address to each pods (dynamic - when the pod stops and new pod is created it will have different IP address than older pod).
- Kube proxy runs on each node and this make sure that is code will get its on unique IP address.

# POD

- Smallest unit of kubernetes.
- Pod is a group of one or more containers that are deployed together on the same host.
- A cluster is a group of nodes.
- A cluster has at least one  worker node and one master node.
- In cubernetes, the control unit is the pod not containers.
- Pods consists of one or more tightly couple containers.
- Pod runs on node which is controlled by Master.
- Kubernetes only knows about pods (doesn't know about individual container).
- Cannot start containers without a pod.
- One pod usually contains one container.

## Multi Container POD

- Share access to memory space.
- Connect to each other using localhost <container port>.
- Share access to the same volume.
- Continuous within poad are deployed in an all-or-nothing manner.
- Entire pod is hosted on the same note (scheduler will decide about which node.

## POD Limitations

- No auto healing her auto scaling.
- Pod crashes quite frequently.
