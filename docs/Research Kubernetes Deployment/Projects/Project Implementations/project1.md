## Basic Kubernetes Cluster Setup and Exploration

### Objective

Set up a basic Kubernetes cluster and explore the master and worker nodes to understand the roles of the key components.

### Steps
1. [Create VMs Using Multipass](https://github.com/sinke237/automated-scripts)
2. Connect to each VM and install `kubeadm`, `kubelet`, and `kubectl`.
    ```shell
    # For each VM, run the following commands:
    
    # Connect to the master node
    multipass shell [vm-name]
    
    # Update the package index
    sudo apt-get update
    
    # Install required packages
    sudo apt-get install -y apt-transport-https ca-certificates curl
    
    # Download Google Cloud public signing key
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    
    # Add Kubernetes APT repository
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    
    # Update the package index again
    sudo apt-get update
    
    # Install kubelet, kubeadm, and kubectl
    sudo apt-get install -y kubelet kubeadm kubectl
    
    # Mark them as held
    sudo apt-mark hold kubelet kubeadm kubectl
    
    # Repeat the above steps for k8s-worker1 and k8s-worker2
    
    ```
3. Initialize the Master Node
    - On the master node VM, run:
    ```shell
    # Connect to the master node
    multipass shell [vm-name]
    
    # Initialize the Kubernetes cluster
    sudo kubeadm init
    
    ```
   - After the initialization completes, you will see a command to join the worker nodes. Note this command.

   - Next, configure `kubectl` access:
      ```shell
     # Set up kubeconfig for kubectl
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config
     ```
4. Join Worker Nodes to the Cluster
   - On each worker node VM, use the command provided by the kubeadm init output. For example:
   ```shell
    # Connect to the first worker node
    multipass shell k8s-worker1
    
    # Join the worker node to the cluster (replace with your actual command)
    sudo kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
    
    # Repeat for the second worker node
    multipass shell k8s-worker2
    sudo kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
    
    ```
5. Verify Cluster Components
   - Back on the master node, verify the cluster status:
   ```shell
    # Connect to the master node
    multipass shell k8s-master
    
    # Check the status of nodes
    kubectl get nodes
    
    # Check the status of all pods in all namespaces
    kubectl get pods --all-namespaces
    
    ```
6. Explore Cluster Components
    - API Server: Interact with the API server using various kubectl get commands. For example:
       ```shell
        kubectl get pods
        kubectl get services
        kubectl get deployments
        
        ```
    - Controller Manager: Observe the status of ReplicaSets and Deployments:
      ```shell
         kubectl get replicasets
         kubectl get deployments
            
      ```
    - Scheduler: Review pod scheduling decisions:
      ```shell
          kubectl describe pod <pod-name>
                
      ```
    - etcd: Understand its role in storing cluster state. You can query etcd if needed, but typically, you would interact with it indirectly through Kubernetes commands.

Conclusion
You have successfully set up a basic Kubernetes cluster with one master node and two worker nodes. You can now explore the various components and their roles within the cluster. If you have any specific questions or need further assistance with any part of the setup, feel free to ask!