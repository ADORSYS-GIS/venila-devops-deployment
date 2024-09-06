
#### **Project 2: Deployment and Service Management**

**Objective**: Deploy a sample application and expose it using Kubernetes Services. Understand how deployments, services, and network routing work.

**Steps**:

1. **Create a Sample Deployment**:
    - Write and apply a YAML configuration for a deployment (e.g., Nginx web server).

2. **Expose the Deployment Using a Service**:
    - Create a Service YAML to expose the Nginx deployment.
    - Types of Services:
        - **ClusterIP**: For internal access within the cluster.
        - **NodePort**: For external access through node IP and port.

3. **Access the Application**:
    - Use `kubectl get services` to find the service details and access the application.

4. **Explore Networking**:
    - Investigate how the service routes traffic to the pods and understand load balancing.

**Outcome**: This project will provide hands-on experience with application deployment and service exposure, including understanding Kubernetes networking.
