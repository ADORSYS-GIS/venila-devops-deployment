
#### **Project 1: Basic Kubernetes Cluster Setup and Exploration**

**Objective**: Set up a basic Kubernetes cluster and explore the master and worker nodes to understand the roles of the key components.

**Steps**:

1. **Create VMs Using Multipass**:
    - [Create three VMs:](https://github.com/sinke237/automated-scripts) one for the master node and two for worker nodes.

2. **Install Kubernetes Components**:
    - On each VM, install `kubeadm`, `kubelet`, and `kubectl`.

3. **Initialize the Master Node**:
    - On the master node VM, run `kubeadm init` to initialize the cluster.
    - Configure `kubectl` access on the master node by setting up the kubeconfig file.

4. **Join Worker Nodes to the Cluster**:
    - On the worker nodes, use the command provided by `kubeadm init` to join the cluster.

5. **Verify Cluster Components**:
    - Use `kubectl get nodes` and `kubectl get pods --all-namespaces` to check the status of cluster components.

6. **Explore Cluster Components**:
    - **API Server**: Use `kubectl get` commands to interact with the API.
    - **Controller Manager**: Observe the status of ReplicaSets and Deployments.
    - **Scheduler**: Review pod scheduling decisions using `kubectl describe pod`.
    - **etcd**: Understand its role in storing cluster state by querying etcd if needed.

**Outcome**: This project will give you hands-on experience with Kubernetes cluster setup and a foundational understanding of how key components interact.

