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

kubectl get deployments
kubectl get pods
kubectl get services
kubectl delete -f nginx-service.yaml
kubectl delete -f nginx-deployment.yaml
kubectl get deployments
kubectl get services


kubectl get deployment nginx-deployment

kubectl scale deployment nginx-deployment --replicas=5

kubectl get deployment nginx-deployment
kubectl get pods -l app=nginx


kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl get deployment -n kube-system metrics-server

// hpa

kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=2 --max=10
kubectl get hpa
kubectl run -i --tty load-generator --image=busybox -- /bin/sh -c "while true; do wget -q -O- http://nginx-service; done"

// test hpa

kubectl run -i --tty load-generator --image=busybox -- /bin/sh -c "while true; do wget -q -O- http://nginx-service; done"
kubectl get hpa -w


// node scaling 

eksctl scale nodegroup \
  --cluster assignment-cluster \
  --name standard-workers \
  --nodes-min 2 \
  --nodes-max 5

eksctl get nodegroup --cluster assignment-cluster

sudo nano stress-deployment.yaml

kubectl get nodes -w

// clean up

kubectl delete hpa nginx-deployment
kubectl delete -f stress-deployment.yaml

// reset node scaling

eksctl scale nodegroup \
  --cluster assignment-cluster \
  --name standard-workers \
  --nodes-min 2 \
  --nodes-max 2

kubectl get hpa
kubectl get nodes -w
```

