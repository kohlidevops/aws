# Amazon Elastic Kubernetes Service

EKS is a managed kubernetes service in AWS cloud.

### Why should we go to Amazon EKS?
 
Traditionally we will create a control plane (Master node) and data plane (Worker node) to manage kubernetes cluster.

If you are creating High available kubernetes cluster, then we can create Multi master nodes and multi worker nodes in any environment.

If we creating kubernetes cluster using kubeadm, then we have to spinup 3 EC2 instance for master node which has components such as API server, ETCD, Scheduler, Controller manager and Cloud controller manager where as worker node has some components such as CNI (Container network interface), container run time, DNS core, and kubeproxy.

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

When you go EKS with Fargate, then you dont need to worry about anything. Because Fargate serverless will take of HA for your woker nodes. So when CPU goes above, sudden spike or if any nodes went down then this Fargate will take care all the things.

##### Amazon EKS is managed control plane whereas Amazon Fargate is managed data plane

So Amazon EKS is robust and Highly available cluster and when you go with EKS then your work is significantly less.



