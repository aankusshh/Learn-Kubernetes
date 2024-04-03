# Kubernetes

Kubernetes is an open source container management tool which automates container deployment, containers scaling and load balancing.

It schedules, runs and manages isolated containers which are running on virtual, physical or cloud machines.

## Features of Kubernetes

- Orchestration (Clustering of any number of containers running on different network)
- Autoscaling, Auto-Healing, Load-Balancing (Vertical and horizontal Scaling)
- Platform independent
- Fault Tolerance (Node/POD Failure)
- RollBack (Going back to previous version)
- Health monitoring of containers
- Batch execution (One time, sequential, parallel)

## Problems without Kubernetes

- Containers cannot communicate with each other.
- Auto Scaling and Load Balancing was not possible (have to take help of higher level objects for it).
- Containers had to be manages carefully.

## Set Up Kubernetes or Minikube

1. Update the `apt` package index and install packages needed to use the Kubernetes `apt` repository:

```bash
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl
```

1. Download the public signing key for the Kubernetes package repositories. The same signing key is used for all repositories so you can disregard the version in the URL:

```bash
# If the folder `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

**Note: In releases older than Debian 12 and Ubuntu 22.04, folder `/etc/apt/keyrings` does not exist by default, and it should be created before the curl command.**

1. Add the appropriate Kubernetes `apt` repository. If you want to use Kubernetes version different than v1.29, replace v1.29 with the desired minor version in the command below:
2. Update `apt` package index, then install kubectl:

```bash
sudo apt-get update
sudo apt install -y kubeadm kubelet kubectl
```

1. Check that kubectl is properly configured by getting the cluster state:

```bash
kubectl cluster-info
```

1. **I**nitialize Kubernetes Master Node

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

1. Set Up Kubernetes Configuration for Current User

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

1. Deploy Networking and Ingress Controllers
    - Deploy Calico Network Plugin

```bash
kubectl apply -f https://docs.projectcalico.org/v3.20/manifests/calico.yaml
```

- Deploy NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml
```

This document provides a step-by-step guide to set up a Kubernetes cluster on an Ubuntu system, including installing Docker, adding Kubernetes repositories, installing Kubernetes components, initializing the master node, configuring Kubernetes for the current user, and deploying essential networking and ingress controllers.