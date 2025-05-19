# Handbook: Setting Up a Kubernetes Cluster with Amazon EKS  

## Scenario Overview  
The team is tasked with preparing infrastructure for a Kubernetes-based application using Amazon EKS. The cluster must meet internal security and scalability standards, use the latest stable Kubernetes version (1.30), and have a private endpoint. It will be deployed in the default VPC across availability zones `a`, `b`, and `c` for high availability.  

---

## Step 1: Prerequisites  

1. **AWS CLI**: Ensure the AWS CLI is installed and configured with appropriate permissions.  
2. **kubectl**: Install `kubectl` to interact with the Kubernetes cluster.  
3. **eksctl**: Install `eksctl` for simplified EKS cluster provisioning.  

---

## Step 2: Create an IAM Role for EKS  

1. Open the AWS Management Console.  
2. Navigate to **IAM > Roles > Create Role**.  
3. Select **AWS Service** and choose **EKS**.  
4. Attach the following policies:  
    - `AmazonEKSClusterPolicy`  
5. Name the role `eksClusterRole` and create it.  

---

## Step 3: Create the EKS Cluster  

Run the following `eksctl` command to create the cluster:  

```bash  
eksctl create cluster \
  --name devops-eks \
  --version 1.30 \
  --region <your-region> \
  --vpc-private-subnets <subnet-ids-for-a,b,c> \
  --node-private-networking \
  --with-oidc \
  --role-arn arn:aws:iam::<account-id>:role/eksClusterRole \
  --zones <region>a,<region>b,<region>c \
  --endpoint-access private
```  

Replace `<your-region>` and `<account-id>` with your AWS region and account ID.  

---

## Step 4: Verify the Cluster  

1. Confirm the cluster is active:  
    ```bash  
    aws eks describe-cluster --name devops-eks --region <your-region> --query "cluster.status"
    ```  

2. Check the endpoint access configuration:  
    ```bash  
    aws eks describe-cluster --name devops-eks --region <your-region> --query "cluster.resourcesVpcConfig.endpointPrivateAccess"
    ```  

3. Test connectivity to the cluster:  
    ```bash  
    kubectl get nodes
    ```  

---

## Step 5: Next Steps  

- Deploy workloads to the cluster.  
- Set up monitoring and logging using Amazon CloudWatch.  
- Implement security best practices, such as network policies and IAM roles for service accounts.  

---  

## Conclusion  

The EKS cluster `devops-eks` is now successfully created with Kubernetes version 1.30, private endpoint access, and high availability across three availability zones. The infrastructure is ready for deploying Kubernetes-based applications.  
