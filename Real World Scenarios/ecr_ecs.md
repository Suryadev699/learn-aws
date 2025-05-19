# Handbook: Deploying a Containerized Application with Amazon ECR and ECS  

## 1. Create a Private ECR Repository  
1. Open the AWS Management Console and navigate to **Amazon ECR**.  
2. Click **Create repository**.  
3. Choose **Private** repository.  
4. Enter the repository name as `ops-ecr`.  
5. Click **Create repository**.  

## 2. Build and Push Docker Image  
### Create a Dockerfile  
Create a `Dockerfile` in your project directory. Example:  
```dockerfile  
FROM nginx:latest  
COPY . /usr/share/nginx/html  
```  

### Build the Docker Image  
Run the following command to build the Docker image:  
```bash  
docker build -t ops-ecr:latest .  
```  

### Authenticate Docker to ECR  
Run the following command to authenticate Docker to your ECR registry:  
```bash  
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com  
```  

### Tag the Docker Image  
Tag the image for the ECR repository:  
```bash  
docker tag ops-ecr:latest <account_id>.dkr.ecr.<region>.amazonaws.com/ops-ecr:latest  
```  

### Push the Docker Image  
Push the image to the ECR repository:  
```bash  
docker push <account_id>.dkr.ecr.<region>.amazonaws.com/ops-ecr:latest  
```  

## 3. Create and Configure ECS Cluster  
1. Open the AWS Management Console and navigate to **Amazon ECS**.  
2. Click **Clusters** > **Create Cluster**.  
3. Select **Networking only (Fargate)**.  
4. Enter the cluster name as `ops-cluster`.  
5. Click **Create**.  

## 4. Create an ECS Task Definition  
1. Navigate to **Task Definitions** > **Create new Task Definition**.  
2. Select **Fargate** as the launch type.  
3. Enter the task definition name as `ops-taskdefinition`.  
4. Add a container:  
    - Container name: `ops-container`.  
    - Image: `<account_id>.dkr.ecr.<region>.amazonaws.com/ops-ecr:latest`.  
    - CPU: Specify required CPU units (e.g., `256`).  
    - Memory: Specify required memory (e.g., `512`).  
5. Click **Create**.  

## 5. Deploy the Application Using ECS Service  
1. Navigate to **Services** > **Create**.  
2. Select the cluster `ops-cluster`.  
3. Enter the service name as `ops-service`.  
4. Set the number of tasks to `1`.  
5. Configure networking:  
    - Select a VPC and subnets.  
    - Enable Auto-assign public IP.  
6. Click **Create Service**.  

## 6. Open Port 80 for Incoming Connections  
1. Navigate to **EC2** > **Security Groups**.  
2. Find the security group associated with the ECS service.  
3. Edit inbound rules:  
    - Add a rule to allow HTTP traffic on port `80` from `0.0.0.0/0`.  
4. Save the changes.  

## Conclusion  
You have successfully deployed a containerized application using Amazon ECR and ECS. The application is now accessible on port 80.  