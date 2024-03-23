# Amazon Elastic Kubernetes Service

EKS is a managed kubernetes service in AWS cloud.

### Why should we go to Amazon EKS?
 
Traditionally we will create a control plane (Master node) and data plane (Worker node) to manage kubernetes cluster.

If you are creating High available kubernetes cluster, then we can create Multi master nodes and multi worker nodes in any environment.

If we creating kubernetes cluster using kubeadm, then we have to spinup 3 EC2 instance for master node which has components such as API server, ETCD, Scheduler, Controller

manager and Cloud controller manager where as worker node has some components such as CNI (Container network interface), container run time, DNS core, and kubeproxy.

However if we create a kubernetes cluster with the help of KOPS, then KOPS tool will create all the components in cluster.

But if you are facing issues like

Any master goes down

Certificate has been expired

API server is not responding or very slow

ETCD has been crashed

Scheduler is not working

then as a DevOps engineer you need to explore and fix the issues. It really very hectic to us.

Think about again, If you are using 1000 of clusters, then how can you manage and fix the problem.

You should monitor all the resources using some monitoring tools and developing some rules to manage it. We definetly need to put more effort to manage all the things. (So it error prone)

### What is Amazon EKS?

Amazon brings managed service that is called EKS.

In EKS, Managed control plane, not managed data plane.

If you go with HA kubernetes cluster, 3 masters for example - But you should not know about where its deployed. Becasue Amazon is going to manage.

For Data plane, Amazon provides easy way to associate worker nodes to kubernetes clusters.

So what are the modes can we use for worker nodes?

1. You can create woker nodes as EC2 instance and associate to EKS cluster
2. You can use Fargate that is Serverless

When you go EKS with EC2 instances you need to create a HA for worker nodes that is Autoscaling with some threshold. If CPU goes above then launch one more woker node for example.

When you go EKS with Fargate, then you dont need to worry about anything. Because Fargate serverless will take of HA for your woker nodes. So when CPU goes above, sudden spike or

if any nodes went down then this Fargate will take care all the things.

##### Amazon EKS is managed control plane whereas Amazon Fargate is managed data plane

So Amazon EKS is robust and Highly available cluster and when you go with EKS then your work is significantly less and we dont need to worry about anything like if API server is

not respoding, Scheduler is not working, Certificate has been expired or ETCD has been crashed in EKS cluster.

Usually, EKS master node wont down. If its happen, then you can talk to AWS team and you may get compensation if SLA is breaching.

Kubernetes will deploy by below ways

1. Kubernetes deployed on cloud using kubeadm or KOPS
2. Amazon EKS
3. Kubernetes deployed on On premise data center

Customers moved from on premise to cloud then moved to EKS cluster.

### How can we deploy application on EKS cluster?

If you have 3 master nodes and 3 worker nodes to manage your application.

1. To begin with, you create pod with the help of kubectl.
2. Usually all pods comes with ClusterIP
3. In addidtion, we can create a service for the pods
4. Furthermore, we create a service, it comes with 3 types
   a. ClusterIP
   b. NodePort
   c. LoadBalancer

ClusterIP - With in cluster you can access the application

NodePort - User can access Who knows the Master node or Worker node IP address.

LoadBalancer - Anyone can access from the outside world.

However, If you are using more clusters then you need more LoadBalancer to grant access to all the users. It would be high pricing.

So we can go with Ingress resource in Kubernetes cluster to avoid more pricing.

### How Ingress resource working in kubernetes cluster?

1. In worker nodes, you are creating pod which comes with cluster IP.
2. In worker nodes, you are creating service
3. Then you are creating Ingress resource which usually works with ClusterIP or NodePort
4. The Ingress resource allow user to access the application inside the kubernetes cluster.
5. The ingress resource route the traffic inside the cluster.
6. DevOps engineer write the Ingress yaml file to allow the users to access the application called demo.com/abc.
7. If someone accessing the demo.com/abc then Ingress resource forward this request to service. From the service, the request forward to Pod.
8. This configuration write on Ingress yaml file and it is deployed by kubectl
9. Nothing will happen, If you are deployed Ingress resource only
10. There has be one who help you to access the public subnet (Loadbalancer for example).
11. From this public faced Loadbalancer, it will access the application inside the private subnet
12. There is concept in Kubernetes called as Ingress controller.
13. Typically all loadbalancers supports Ingress controller such as Nginx, F5.
14. You can deploy those Ingress controller with the help of Helm chart or plain mainfest yaml file.
15. If you are deploying Ingress controller for AWS ALB, as soon as the DevOps engineer create a Ingress resource in cluster.
16. Then ALB Ingress controller watch the Ingress resource then it will create ALB for you.
17. Now user can access the application with the help of Application Loadbalancer.

#### Im going to use AWS CloudShell to install EKS cluster

### How to install kubectl on Amazon Linux2023?

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### How to install eksctl on Amazon Linux2023?

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

### How to Install AWS CLI on Amazon Linux2023?

Bydefault, it is installed on CloudShell

### How to create a kubernetes cluster using eksctl?

```
eksctl create cluster --name demo-cluster-1 --region us-east-2 --fargate
```

![image](https://github.com/kohlidevops/aws/assets/100069489/2dba23b2-b663-4678-829a-3d6c5fc796f2)

![image](https://github.com/kohlidevops/aws/assets/100069489/1f7cfd40-5394-437b-a8c1-c630b8519613)

My demo cluster has been created!

![image](https://github.com/kohlidevops/aws/assets/100069489/d3aece85-2802-4158-9feb-51f6071a2ca9)

### How to update the kubeconfig on to your cloudshell?

```
aws eks update-kubeconfig --name demo-cluster-1 --region us-east-2
```

### How to create a new namespace for fargate?

By default, fargate profile created with default and kube-system. 

![image](https://github.com/kohlidevops/aws/assets/100069489/50a5cda9-97b2-4bd2-88e6-d5c4c554c2b0)

But we can create a namespace for fargate profile.

```
eksctl create fargateprofile \
    --cluster demo-cluster-1 \
    --region us-east-2 \
    --name alb-sample-app \
    --namespace game-2048
```

![image](https://github.com/kohlidevops/aws/assets/100069489/71c34884-d4a9-4858-b1de-1cf7bbe8cdea)

You can see under compute in EKS cluster. One more prfile has been created.

![image](https://github.com/kohlidevops/aws/assets/100069489/f3ae3c70-0971-41d3-806b-2c5b5e4d1f80)

### To deploy the Deployment, Service and Ingress

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

Now Ingress only created not Ingress controller. So nothing will happen until the Ingress controller creation.

![image](https://github.com/kohlidevops/aws/assets/100069489/f46cdb72-ee4e-45b5-9a42-4e4e3d604e96)

All pods are running

```
kubectl get pods -n game-2048 -w
```

![image](https://github.com/kohlidevops/aws/assets/100069489/f99e49c3-6d30-41fd-bfc6-7350fa77fc02)

To check the service

```
kubectl get svc -n game-2048
```

![image](https://github.com/kohlidevops/aws/assets/100069489/180b742f-ba71-483b-90c2-bd20fa006291)

To check the Ingress

```
kubectl get ingress -n game-2048
```

![image](https://github.com/kohlidevops/aws/assets/100069489/573514d9-c5b5-4b6f-a59f-6bfe0f73616a)

Address is null in above image - Because we are still not invoke ingress controller

To deploy the IAM OIDC provider

why do we need this? - If pod want to communicate with any aws services then it need IAM permission. Thats why we have to deploy the IAM OIDC provider before deploying Ingress controller.

```
eksctl utils associate-iam-oidc-provider --cluster demo-cluster-1 --approve
```

To download the IAM policy

```
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

To create a IAM policy

```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

To create an IAM role

```
eksctl create iamserviceaccount \
  --cluster=demo-cluster-1 \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

To add the Helm repo

Install Helm if doesn't exist

```
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

Now add the repo

```
helm repo add eks https://aws.github.io/eks-charts
```

To update the repo

```
helm repo update eks
```

To install

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \
  --set clusterName=demo-cluster-1 \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=us-east-2 \
  --set vpcId=vpc-0c9211d59da1215e6
```

You can find the VPC ID from EKS cluster - Networking

![image](https://github.com/kohlidevops/aws/assets/100069489/5ce3f9e1-eb1f-4650-89d7-d0f41b6baf74)

AWS ALB controller installed!

To verify the deployment is running

```
kubectl get deployment -n kube-system aws-load-balancer-controller
```

![image](https://github.com/kohlidevops/aws/assets/100069489/91dd47e2-693f-42e0-a29d-824d3deb3851)

The Application Loadbalancer has been created

![image](https://github.com/kohlidevops/aws/assets/100069489/71246a08-beba-46fc-8bd4-29e292bfc566)

To check the ingress now

```
kubectl get ingress -n game-2048
```

![image](https://github.com/kohlidevops/aws/assets/100069489/8dde1e17-5cc0-412f-a6ac-a5e2d8c2167b)

Now address is available - Because the ALB Ingress controller created ALB

If I check with the ALB DNS, then i can see the 2048 game

![image](https://github.com/kohlidevops/aws/assets/100069489/b81369ac-90ec-4034-82ba-2deaded5c4c0)

That's it!




