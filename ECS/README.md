# Amazon Elastic Container Service (ECS)

### What is Docker?

It is a container platform which allows us to manage the lifecycle of container.

### Why do we need ECS or Kubernetes?

If you are installed docker on VM and running container on top of it then your application will be expose to the external world.

Then why do we need to go with ECS or Kubernetes?

Docker doesn't has the ability of Autoscaling and Autohealing by nature.

If someone is accidentally deleted any container, then new container will not up and running until you initiate the process. Thus docker doesn't have the ability of autohealing.

If application getting loads more, then more container will not up and running. Thus docker doesn't have the ability of autoscaling.

Because Docker container is ephemeral - Anytime can down for many reasons.

So definetly customer facing issues while accessing the sites.

Thats why Container Orchestration Engines (ECS or Kubernetes) comes into the picture.

### What is Amazon ECS?

Amazon brings their own container orchestartion called as Elastic Container Service which has below components.

1. Tasks
2. Task Definition
3. Services
4. Clusters

It is bound with AWS Cloud only.

It is not open source platform orchestration service.

AWS Elastic Container Service is a fully managed container orchestration service that allows us to run a Docker containers. It eliminates the need to manage your own container orchestration infrastructure and provides a highly scalable, reliable, and secure environment for deploying and managing your applications.

### Benefits of ECS

1. Fully managed service
2. Integrated with IAM, CloudWatch, LoadBalancer
3. Scalability
4. Cost effective

### Drawbacks of ECS

1. Tigthly integrated with AWS - If you plan to move to another cloud then we can't reuse this ECS again.
2. Advanced features might required deep understanding
3. ECS is good container orchestration, but it doesn't has all the features which does kubernetes.

### ECS VS EKS

1. ECS has two types - EC2 Server and Fargate
2. When you go ECS with Fargate then you dont need to worry about anything. This things will take care of all the things.
3. When you go ECS with EC2 server then you have to monitor the EC2 server frequently.
4. ECS is AWS proprietary whereas EKS is kubernetes that is open source.
5. In kubernetes we called as pod whereas we called Task in ECS.
6. In EKS, we have to manage Dataplane if we are not using worker node as Fargate type.
7. In Kubernetes, we have to manage both Controlplane and Dataplane & its components too.
8. But ECS is very simple, that need to create a task and task definition then you need to create a service which integrate with loadbalancer.

### Fundamental of ECS

#### what is Clusters?

It is a is a logical grouping of EC2 instances or Fargate tasks on which you run your containers. It acts as the foundation of ECS, where you can deploy your services.

#### What is Task Definitions?

It define how your containers should run, including the Docker image to use, CPU and memory requirements, networking, and more. It is like a blueprint for your containers.

#### What is a Tasks?

It could be a single container or multiple related containers that need to work together.

#### What is Services?

It helps us to maintain a specified number of running tasks simultaneously, ensuring high availability and load balancing for your applications.

