# Amazon Elastic Container Regisrty (ECR)

## What is Amazon ECR?

It is a container registry similiar to Docker hub.

It is using to store the docker images in Amazon Cloud. 

If you have a docker image and you want to share it with one from around the world then you can store this image on to any registry and you can share this image URL. This is called a container registry.

Then what does Elastic mean? - It means Highly available and Scalable in AWS Cloud. We can store any number of images on it and Amazon assuring you that container registry is always available.

### ECR vs DockerHub Regisry

DockerHub is free login and by default, the public repository will be created. Anyone can access the public repository.

DockerHub can support private repositroy. Authorized user can access the private repository.

ECR is a AWS Cloud based login and by default, the private repository will be created. So Authorized user can use it.

Amazon ECR is primarily focusing private repository and its increasing security postures.

However, Amazon ECR will support public repository too.

If your organization using DockerHub for storing image then your 'x' number of users need to login and store the image.

What will happen If DockerHub went down? - Definetly your day is very hectic.

If your orgainzation using AWS ECR for storing image then you can use IAM user with required policy to access the ECR.

What will happen if your region where ECR is deployed went down? - We can write replication rule to replicate our private images to some other destination regions. Its highly available.

This ECR is well integrated and matured relationship with other AWS services such as Elastic Container Service, Elastic Kubernetes Service and Fargate like.

You can write the Lifecycle policies to automate the cleanup of unused or old images and thus helping you to reduce the cost.

SO what are the Key benefits of using ECR?

1. Security
2. Integration
3. Highly Available
4. Scalable
5. Replication
6. Lifecycle policy

To be honest, DockerHub is mainly preferred for public based images, whereas ECR is mainly using for storing private images.

If you are in Cloud native, then as a DevOps engineer you should go with ECR than DockerHub.

### How to create a ECR in Amazon Cloud?

Navigate to AWS ECR and select - Get started

![image](https://github.com/kohlidevops/aws/assets/100069489/38a24d15-7b8e-4195-ba65-b2cb202a5f03)

![image](https://github.com/kohlidevops/aws/assets/100069489/aefee2c1-55c3-4291-8cdd-0d8c215ce3fe)

You can use Scan on Push option to image scanning and enable KMS encryption too.

![image](https://github.com/kohlidevops/aws/assets/100069489/c8d6e22c-b26e-4242-8501-a216a6dff62c)

Then create a repository.

![image](https://github.com/kohlidevops/aws/assets/100069489/8e90d84a-7e4b-4692-951d-b93faba2d51d)

You can use "view push commands" option to know how to push the image on to ECR.

### How to push the image to AWS ECR?

Im using CloudShell to execute push commands - So I dont need to configure aws on it.

I just create one docker file

```
sudo vi Dockerfile
```

below content

```
FROM ubuntu:latest
```

To begin with, you need to logon to ECR

```
aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-2.amazonaws.com
```

In addition, you have to build the docker image using simple Dockerfile

```
docker build -t latchudevops .
```

![image](https://github.com/kohlidevops/aws/assets/100069489/581cf75a-23ef-4c5e-b838-20b7b5cfd66a)

Further more, you need to Tag your image

```
docker tag latchudevops:latest <account-id>.dkr.ecr.us-east-2.amazonaws.com/latchudevops:latest
```

Finally push the docker image to the ECR repository

```
docker push <account-id>.dkr.ecr.us-east-2.amazonaws.com/latchudevops:latest
```

![image](https://github.com/kohlidevops/aws/assets/100069489/38b67559-afa3-450b-b6d7-271401a4abc4)

If you want to take a look at the AWS ECR repository, then you can see your private images

![image](https://github.com/kohlidevops/aws/assets/100069489/6910b1d7-b431-49d9-929b-5f452b7987a3)

That's it.


