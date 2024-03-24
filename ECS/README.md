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

### How to deploy application on Amazon ECS

Sample application is available in below link

```
https://github.com/kohlidevops/aws/tree/main/ECS
```

Logon to AWS console and Navigate to ECS dashboard - Create Cluster

![image](https://github.com/kohlidevops/aws/assets/100069489/fc2bf6d2-9f80-4c0e-a4f1-5ef77a683a0e)

![image](https://github.com/kohlidevops/aws/assets/100069489/71b168a1-7061-40e7-873d-69c102eeb7ed)

Then - Create a custer.

#### To create a ECR registry

You can use below link to create and configure the ECR in AWS

```
https://github.com/kohlidevops/aws/blob/main/ECR/README.md
```

![image](https://github.com/kohlidevops/aws/assets/100069489/ef839920-6c23-4bf3-ade2-20a21f5b533f)

Im using Cloudshell to push the application to ECR and placed my files inside CLoudShell

![image](https://github.com/kohlidevops/aws/assets/100069489/270be422-748a-41a1-9a3d-de7fcfe79af1)

To logon to ECR

```
aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-2.amazonaws.com
```

To build the docker image

```
docker build -t latchudevops .
```

To tag your image before pushing

```
docker tag latchudevops:latest <account-id>.dkr.ecr.us-east-2.amazonaws.com/latchudevops:latest
```

To push the docker image to ECR

```
docker push <account-id>.dkr.ecr.us-east-2.amazonaws.com/latchudevops:latest
```

Docker image has been pushed onto ECR

![image](https://github.com/kohlidevops/aws/assets/100069489/3696b2b4-a347-4d8a-b204-8bcbe86da857)

#### To create a Task definition in ECS cluster

Already we have created ECS cluster. Now I need to create a Task definition.

![image](https://github.com/kohlidevops/aws/assets/100069489/aca0d614-e9c3-4441-a2b2-b507371fd0e6)

![image](https://github.com/kohlidevops/aws/assets/100069489/4e6dc528-6bec-4499-84db-3d59e6964c9b)

![image](https://github.com/kohlidevops/aws/assets/100069489/4b8b4929-ae05-4a0f-8f7d-7e5c61912674)

![image](https://github.com/kohlidevops/aws/assets/100069489/f83f680c-7145-46fc-818c-b820abc87535)

Time to feed the container details.

![image](https://github.com/kohlidevops/aws/assets/100069489/84a76350-4c68-4d82-bf34-6b1a1fb3c0af)

If we integrate with cloudwatch logs, then we can start to analyse with cloudwatch logs if any issues occurred.

![image](https://github.com/kohlidevops/aws/assets/100069489/1e496e73-dec0-4467-ab31-d6e539c227ca)

Finally create a Task definition.

![image](https://github.com/kohlidevops/aws/assets/100069489/15a7f61b-6208-469b-a799-2b514bfea188)

Task definition has been created.

#### To run a Task

Launch type should be Fargate

![image](https://github.com/kohlidevops/aws/assets/100069489/a9323471-0bef-469c-8aaa-983b2e201e68)

Go to Task definition - select - your Task definition - select - Deploy - Run a Task

![image](https://github.com/kohlidevops/aws/assets/100069489/3c4e652d-bbb9-4d2a-ad68-8a6378c57402)

If you want override any configuration then do changes and create a Task

![image](https://github.com/kohlidevops/aws/assets/100069489/355d1d3e-d3fa-4e03-851d-cc43807f48ce)

It is running state. If you can check with cloudwatch logs

![image](https://github.com/kohlidevops/aws/assets/100069489/6748b553-669e-4f39-90fd-962b7e3ac23e)

That's it
