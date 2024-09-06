# Project 2: Deployment and Service Management

## Objective

Deploy a sample application and expose it using Kubernetes Services. Understand how deployments, services, and network routing work.

## Steps

### 1. Create a Sample Deployment

   We'll create a deployment for an Nginx web server. First, create a YAML file for the deployment.

### Create Deployment YAML

Create a file named `nginx-deployment.yaml`:
```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```
### Apply the Deployment

Use the following command to apply the deployment:
```shell
kubectl apply -f nginx-deployment.yaml

```

### 2. Expose the Deployment Using a Service

Now, we will create a Service to expose the Nginx deployment.

### Create Service YAML

Create a file named nginx-service.yaml:
```shell
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort  # Change this to ClusterIP if you want internal access only
  selector:
    app: nginx
  ports:
    - port: 80         # Port that the service will expose
      targetPort: 80   # Port on the container
      nodePort: 30001   # Optional: specify a NodePort

```
### Apply the Service

Use the following command to apply the service:

```shell
kubectl apply -f nginx-service.yaml

```

### 3. Access the Application

After creating the service, you can check its details and access the application.

### Get Service Details

Run the following command to get the service details:
```shell
kubectl get services

```
This command will show you the service name, type, cluster IP, external IP (if applicable), ports, and more.

### Access the Application

- If you used NodePort, you can access the application using the Node's IP and the specified NodePort (e.g., http://<node-ip>:30001).
- If you used ClusterIP, you can only access it from within the cluster.

### 4. Explore Networking

To investigate how the service routes traffic to the pods, you can use the following commands:

### Check Pods
```shell
kubectl get pods

```

### Describe the Service

To understand how the service is routing traffic to the pods, use:
```shell
kubectl describe service nginx-service

```

This will show you the endpoints that the service is routing to, which are the pods created by the deployment.

### Test Load Balancing

You can test load balancing by sending requests to the service multiple times and checking which pod is serving the requests:
```shell
# Get the list of pods
kubectl get pods -l app=nginx

# Use curl to send requests (replace <node-ip> with your actual node IP)
for i in {1..10}; do curl http://<node-ip>:30001; done

```

## Outcome

By completing these steps, you will have deployed a sample Nginx application and exposed it using Kubernetes Services. You will also have gained hands-on experience with:

- Creating and managing deployments.
- Exposing applications using different types of services (ClusterIP and NodePort).
- Understanding how Kubernetes networking and load balancing work.

If you have any questions or need further assistance with any part of the implementation, feel free to ask!