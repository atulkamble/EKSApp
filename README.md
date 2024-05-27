# EKSDeployment
EKS Deployment on AWS

## Use Code
```
git clone https://github.com/atulkamble/EKSDeployment.git
cd EKSDeployment
```

// eksctl

Tip: Open Cloud Shell and Use to save time

Ref: https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html

1) // install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

2) // Create EKS Cluster
eksctl create cluster \
--name mycluster \
--version 1.30 \
--region us-east-1 \
--nodegroup-name linux-nodes \
--node-type t2.micro \
--nodes 2

3) // Try Deployment and create services as well

In Kubernetes, deployments and services are fundamental concepts used to manage and expose your application. Below is a basic overview of each, including syntax and explanations.

### Deployment

A Deployment in Kubernetes ensures that a specified number of instances of an application (Pods) are running at all times. Deployments manage the creation and updates of these Pods.

**Basic Deployment YAML:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image:1.0
        ports:
        - containerPort: 80
```

**Explanation:**
- `apiVersion`: Specifies the API version, here it's `apps/v1`.
- `kind`: Specifies the type of resource, in this case, `Deployment`.
- `metadata`: Contains the name and labels for the deployment.
  - `name`: The name of the deployment.
  - `labels`: Labels to categorize the deployment.
- `spec`: Defines the desired state of the deployment.
  - `replicas`: Number of Pod replicas to run.
  - `selector`: Defines how the deployment finds the Pods it should manage.
    - `matchLabels`: The label selector for the Pods.
  - `template`: Describes the Pod that will be created.
    - `metadata`: Contains labels for the Pods.
    - `spec`: Defines the containers within the Pod.
      - `containers`: Specifies the container configuration.
        - `name`: The name of the container.
        - `image`: The container image to run.
        - `ports`: The ports to expose from the container.

### Service

A Service in Kubernetes defines a logical set of Pods and a policy to access them. Services enable network access to a set of Pods.

**Basic Service YAML:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  labels:
    app: my-app
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

**Explanation:**
- `apiVersion`: Specifies the API version, here it's `v1`.
- `kind`: Specifies the type of resource, in this case, `Service`.
- `metadata`: Contains the name and labels for the service.
  - `name`: The name of the service.
  - `labels`: Labels to categorize the service.
- `spec`: Defines the desired state of the service.
  - `selector`: Defines how the service finds the Pods it should route to.
    - `app`: The label selector for the Pods.
  - `ports`: Specifies the network ports exposed by the service.
    - `protocol`: The protocol used by the port (default is TCP).
    - `port`: The port on which the service is exposed.
    - `targetPort`: The port on the Pod to which traffic should be forwarded.
  - `type`: Specifies the type of service (e.g., `ClusterIP`, `NodePort`, `LoadBalancer`, `ExternalName`).

### Example Explanation

Suppose you have a Deployment named `my-deployment` that manages Pods running your application, each exposing port 80. A Service named `my-service` can then be used to expose these Pods to other components within the cluster or to external clients.

1. **Deployment** ensures there are always 3 Pods running your application.
2. **Service** provides a stable endpoint (IP address) to access these Pods, routing traffic to any of the Pods labeled with `app: my-app`.

This setup abstracts the underlying Pod details, allowing you to focus on deploying and scaling your application while Kubernetes handles the rest.

### Additional Concepts

- **Selectors and Labels:** Selectors are used to identify a set of Pods based on labels. This allows Services and Deployments to target specific Pods.
- **Replicas:** The number of Pod instances a Deployment should maintain.
- **Container Ports:** The port on which the container listens inside the Pod.
- **Service Types:** 
  - `ClusterIP`: Exposes the Service on a cluster-internal IP.
  - `NodePort`: Exposes the Service on each Node's IP at a static port.
  - `LoadBalancer`: Creates an external load balancer (if supported by the cloud provider).
  - `ExternalName`: Maps the Service to the contents of the `externalName` field (e.g., `example.com`).

By understanding and utilizing Deployments and Services, you can effectively manage and scale applications in a Kubernetes cluster.

4) Access Pods, Nodes and Namespaces

kubectl get nodes
kubectl get pods
kubectl get ns

5) To delete all deployments and services in a Kubernetes cluster, you can use the following commands:

1. Delete Deployments:
```bash
kubectl delete deployment --all
```

2. Delete Services:
```bash
kubectl delete service --all
```

3. Delete pods:
```bash
kubectl delete pod --all
```

These commands will delete all deployments and services in the current Kubernetes namespace. If you want to delete resources across all namespaces, you can use the `--all-namespaces` flag:

```bash
kubectl delete deployment --all --all-namespaces
kubectl delete service --all --all-namespaces
```

Please be cautious when running these commands as they will remove all deployments and services, potentially impacting your running applications. Make sure to confirm before proceeding with the deletion.


6) // Delete Cluster

```
eksctl delete cluster --name mycluster
```
## History
```
    1  eksctl create cluster --name mycluster --version 1.17 --region us-east-1 --nodegroup-name linux-nodes --node-type t2.micro --nodes 2
    2  curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
    3  sudo mv /tmp/eksctl /usr/local/bin
    4  eksctl version
    5  eksctl create cluster --name mycluster --version 1.17 --region us-east-1 --nodegroup-name linux-nodes --node-type t2.micro --nodes 2
    6  eksctl create cluster --name mycluster --version 1.30 --region us-east-1 --nodegroup-name linux-nodes --node-type t2.micro --nodes 2
    7  kubectl get nodes
    8  kubectk get pods
    9  kubectl get pods
   10  kubectl get ns
   11  eksctl delete cluster --name mycluster
   12  clear
   13  curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   14  sudo mv /tmp/eksctl /usr/local/bin
   15  eksctl version
   16  eksctl create cluster --name mycluster --version 1.30 --region us-east-1 --nodegroup-name linux-nodes --node-type t2.micro --nodes 2
   17  kubectl get nodes
   18  kubectl get pods
   19  kubectk get ns
   20  touch deployment.yaml
   21  nano deployment.yaml
   22  kubectl apply -f deployment.yaml
   23  kubectl apply -f service.yaml
   24  touch service.yaml
   25  nano service.yaml
   26  kubectl apply -f service.yaml
   27  kubectl get pods
   28  kubectl get services
   29  kubectl delete deployment --all
   30  kubectl delete service --all
   31  kubectl delete pod --all
   32  eksctl delete cluster --name mycluster
   33  history
```
