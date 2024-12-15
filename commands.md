```
aws --version

kubectl version --client

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

eksctl create cluster \
--name mycluster \
--version 1.31 \
--region us-east-1 \
--nodegroup-name linux-nodes \
--node-type t2.medium \
--nodes 2

kubectl get nodes

touch nginx-deployment.yaml
sudo nano nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl get deployments
kubectl get pods -l app=nginx

touch nginx-service.yaml
sudo nano nginx-service.yaml
kubectl apply -f nginx-service.yaml
kubectl get services

// open browser
http://ab86d441fbf5741fd9f3a067a07f3e92-876380523.us-east-1.elb.amazonaws.com/
```

