# Investigation: Auto-Scaling Strategies for Docker Containers

This document investigates common auto-scaling solutions for Docker containers, describing how each solution works, with configuration examples.

## 1. Common Auto-Scaling Solutions

### 1.1 Docker Swarm [Visisit officatial site](https://docs.docker.com/reference/cli/docker/service/scale/)

- **Description:** Docker Swarm is Docker's native clustering and orchestration tool that allows for auto-scaling of containers across a cluster of Docker Engine instances.
- **How It Works:**
  - Docker Swarm uses a manager-worker model, where a manager node handles orchestration, scheduling, and cluster management, while worker nodes run the containers.
  - Auto-scaling in Docker Swarm can be achieved by configuring the number of replicas for a service. When a service needs to scale, the manager node will start additional replicas or remove them based on demand.
- **Configuration Example:**
  ```bash
  # Initialize Docker Swarm
  docker swarm init

  # Create a service with 3 replicas
  docker service create --name my-service --replicas 3 my-docker-image

  # Scale the service to 5 replicas
  docker service scale my-service=5
  ```

- Pros:
  - Integrated directly with Docker Engine.
  - Simple setup and easy to use for smaller deployments.
- Cons:
  - Limited in advanced features compared to Kubernetes.
  - Scaling decisions must be manually triggered or scripted.

- **1.2 Kubernetes with Horizontal Pod Autoscaler (HPA)**
  - **Description:** Kubernetes is a popular container orchestration platform that provides advanced auto-scaling capabilities, including Horizontal Pod Autoscaler (HPA), Vertical Pod Autoscaler (VPA), and Cluster Autoscaler.
  - **How It Works:**
     - **The Horizontal Pod Autoscaler (HPA)** automatically scales the number of pod replicas in a deployment or replica set based on CPU utilization or other custom metrics.
    - **Vertical Pod Autoscaler (VPA)** adjusts the CPU and memory requests and limits of containers.
    - **Cluster Autoscaler** adds or removes nodes from a cluster based on the pending pods that cannot be scheduled due to resource constraints.

- Pros:
  - Highly flexible and customizable.
  - Supports multiple metrics for scaling.
- Cons:
  - Requires a Kubernetes cluster setup.
  - More complex to manage than Docker Swarm.

- **Configuration Example (HPA):**
#### Step 1: Deploy a Docker Container in Kubernetes
First, let's deploy a simple Docker container using a Kubernetes Deployment. For this example, we'll use an Nginx web server.

**1. Create a Deployment YAML file** (nginx-deployment.yaml):
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
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
**2. Apply the Deployment:**

Run the following command to create the deployment in your Kubernetes cluster:
```bash
kubectl apply -f nginx-deployment.yaml
```
**3. Verify the Deployment:**

Check the status of the pods created by the deployment:
```bash
kubectl get deployments
kubectl get pods
```

#### Step 2: Expose the Deployment as a Service
To access the Nginx deployment, we need to expose it using a Kubernetes Service.

**1. Create a Service YAML file (nginx-service.yaml):**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

**2. Apply the Service:**

Run the following command to expose the deployment:

```bash
kubectl apply -f nginx-service.yaml
```

**3. Verify the Service:**
Check the status of the service to ensure it's running:

```bash
kubectl get svc nginx-service
```

### Step 3: Set Up Horizontal Pod Autoscaler (HPA)

#### Create the HPA configuration in YAML format

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-deployment
  namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

**Apply the HPA Configuration**

**1. Save the HPA Configuration:** Save the above YAML content to a file named ``nginx-hpa.yaml``.

**2. Apply the HPA Configuration:** Run the following command to create the Horizontal Pod Autoscaler in your Kubernetes cluster:
```bash
kubectl apply -f nginx-hpa.yaml
```
**Verify the HPA**

Check the status of the HPA to ensure it is correctly set up:
```bash
kubectl get hpa
```
**Step 4: Test Auto-Scaling**

To test the auto-scaling functionality, you can simulate a high CPU load on the Nginx pods using tools like ``stress`` or ``ab`` (Apache Benchmark).

**Generate Load**
Use a load-testing tool like ab (Apache Benchmark) to generate load:
```bash
ab -n 100000 -c 100 http://<external-ip-of-nginx-service>/
```
Replace ``<external-ip-of-nginx-service>`` with the external IP of the Nginx service obtained from:
```bash
kubectl get svc nginx-service
```
**- Monitor the HPA**

Watch the HPA status to see how it scales the deployment up or down based on the load:
```bash
kubectl get hpa -w
```
#### Step 5: Clean Up
To remove all the resources created for this example, run:
```bash
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml
kubectl delete -f nginx-hpa.yaml
```
**Conclusion**

This example demonstrates how to deploy a Docker container (Nginx) in Kubernetes, expose it via a LoadBalancer service, and set up auto-scaling using the Kubernetes Horizontal Pod Autoscaler (HPA). By simulating a load, you can observe the HPA dynamically adjusting the number of replicas to maintain a stable environment.

## 1.3 AWS Auto Scaling with ECS (Elastic Container Service)

**Description:**  
AWS ECS provides native integration with AWS Auto Scaling, allowing for dynamic scaling of containerized applications based on demand.

### How It Works:
- ECS services can be configured to use auto-scaling policies based on CloudWatch metrics (e.g., CPU and memory usage).
- ECS also supports scaling at the container level (Task Scaling) and infrastructure level (Service Auto Scaling).

### Configuration Example:
1. **Create a scaling policy in the ECS console:**
   - Navigate to the ECS service in AWS Management Console.
   - Choose your cluster and service.
   - Configure auto-scaling based on your desired metrics (e.g., CPU utilization above 70%).

### Pros:
- Deep integration with AWS services.
- Supports both container-level and infrastructure-level scaling.

### Cons:
- Vendor lock-in to AWS.
- Costs associated with AWS services.

## Step-by-Step Guide to Set Up AWS Auto Scaling with ECS

### 1. Prerequisites:
- An AWS account.
- An ECS cluster set up with at least one running ECS service.
- A CloudWatch alarm to monitor CPU utilization (we'll create one in this example).

### 2. Create an ECS Service

**Navigate to ECS:**
- Open the [AWS Management Console](https://aws.amazon.com/console/) and navigate to **ECS**.

**Create or Select a Cluster:**
- Choose an existing ECS cluster or create a new one.

**Create a Service:**
- Choose **Create Service**.
- Select **Launch Type**: Choose **Fargate** or **EC2** based on your infrastructure.
- Choose a **Task Definition**: Select a pre-existing task definition or create a new one.
- Configure the service: Set the **number of tasks** to a starting value (e.g., 2).

**Create the Service:**
- Click **Next Step** and continue with the service creation wizard.

### 3. Configure Auto Scaling for ECS Service

**Open the ECS Service Configuration:**
- Navigate to **Clusters > [Your Cluster] > Services**.
- Select the service you created.

**Configure Auto Scaling:**
- Choose the **Auto Scaling** tab.
- Click on **Edit** to set up auto-scaling policies.

**Create an Auto Scaling Policy:**

- **Target Tracking Scaling Policy:**
  - Select **Target Tracking**.
  - For **Metric type**, choose **ECS Service Average CPU Utilization**.
  - Set **Target value** to 70 (for example, to scale when CPU utilization is above 70%).
  - Set the **Minimum Capacity** to 2 and **Maximum Capacity** to 10.

**Save Changes:**
- Click **Update** to apply the auto-scaling policy.

### 4. Create a CloudWatch Alarm for CPU Utilization

**Navigate to CloudWatch:**
- Open the [AWS Management Console](https://aws.amazon.com/console/) and navigate to **CloudWatch**.

**Create an Alarm:**
- Go to **Alarms > Create Alarm**.
- Select **ECS Service Metrics** and choose **CPUUtilization**.
- Set the condition to trigger when the CPU utilization is greater than 70% for 5 consecutive minutes.

**Configure Actions:**
- Choose **Add Action** and select **Auto Scaling policy** to add the scaling action.

**Complete the Alarm Creation:**
- Click **Create Alarm**.

### 5. Verify Auto Scaling
- Monitor your ECS service and CloudWatch alarms.
- To test, you can generate CPU load in your ECS tasks by running CPU-intensive processes.

### 6. Clean Up Resources
- To avoid incurring costs, delete the ECS service, cluster, and CloudWatch alarm after testing.

### Conclusion:
This practical example demonstrates how to set up AWS Auto Scaling for an ECS service. By using a target tracking scaling policy, the ECS service dynamically scales the number of running tasks based on CPU utilization.


## 1.4 Custom Scripts

**Description:**  
Custom scripts provide a flexible way to implement auto-scaling by using monitoring tools, APIs, and cloud providers' SDKs.

### How It Works:
- A custom script can periodically check the metrics (CPU, memory, or custom metrics) of the Docker containers and decide whether to scale up or down.
- These scripts can use Docker CLI commands, Docker APIs, or cloud provider APIs to adjust the number of running containers.

### Configuration Example:

```bash
# Bash script example to scale Docker containers based on CPU usage
CPU_THRESHOLD=70
CURRENT_CPU=$(docker stats --no-stream --format "{{.CPUPerc}}" my-container | tr -d '%')
if [ "$CURRENT_CPU" -gt "$CPU_THRESHOLD" ]; then
    # Scale up by adding another container
    docker run -d my-docker-image
fi
```

**Pros:**

- Highly customizable and can be tailored to specific needs.
- No need for additional orchestration tools.

**Cons:**
- Requires manual setup and maintenance.
- Lack of advanced features and resilience found in other solutions.