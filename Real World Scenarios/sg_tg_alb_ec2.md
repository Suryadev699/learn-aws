# Handbook: Setting Up Security Group, EC2 Instance, ALB, and Target Group  

## 1. Create a Security Group  

### Via Console  
1. Open the **Amazon EC2 Console**.  
2. Navigate to **Security Groups** under the **Network & Security** section.  
3. Click **Create Security Group**.  
4. Set the **Security group name** to `devops-sg`.  
5. Add an **Inbound Rule**:  
    - Type: HTTP  
    - Protocol: TCP  
    - Port Range: 80  
    - Source: Custom (select the default security group).  
6. Click **Create Security Group**.  

### Via CLI  
```bash  
aws ec2 create-security-group --group-name devops-sg --description "Security group for DevOps"  
aws ec2 authorize-security-group-ingress --group-name devops-sg --protocol tcp --port 80 --source-group <default-security-group-id>  
```  

---

## 2. Create an EC2 Instance  

### Via Console  
1. Open the **Amazon EC2 Console**.  
2. Click **Launch Instance**.  
3. Choose an **Ubuntu AMI** (e.g., Ubuntu Server 20.04 LTS).  
4. Select an instance type (e.g., t2.micro).  
5. Configure the instance:  
    - Add the following **User Data** script:  
      ```bash  
      #!/bin/bash  
      apt update  
      apt install -y nginx  
      systemctl start nginx  
      ```  
6. Attach the `devops-sg` security group to the instance.  
7. Launch the instance and name it `devops-ec2`.  

### Via CLI  
```bash  
aws ec2 run-instances --image-id <ubuntu-ami-id> --count 1 --instance-type t2.micro --key-name <key-pair-name> --security-group-ids <devops-sg-id> --user-data file://user-data.sh --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'  
```  
Create a `user-data.sh` file with the following content:  
```bash  
#!/bin/bash  
apt update  
apt install -y nginx  
systemctl start nginx  
```  

---

## 3. Set Up an Application Load Balancer  

### Via Console  
1. Open the **Amazon EC2 Console**.  
2. Navigate to **Load Balancers** under the **Load Balancing** section.  
3. Click **Create Load Balancer** and select **Application Load Balancer**.  
4. Set the **Name** to `devops-alb`.  
5. Choose **Internet-facing** and **IPv4**.  
6. Add a listener:  
    - Protocol: HTTP  
    - Port: 80  
7. Select the default VPC and subnets.  
8. Attach the **default security group**.  
9. Click **Create Load Balancer**.  

### Via CLI  
```bash  
aws elbv2 create-load-balancer --name devops-alb --subnets <subnet-ids> --security-groups <default-security-group-id> --scheme internet-facing --type application  
```  

---

## 4. Create a Target Group  

### Via Console  
1. Navigate to **Target Groups** under the **Load Balancing** section.  
2. Click **Create Target Group**.  
3. Set the **Name** to `devops-tg`.  
4. Choose **Instances** as the target type.  
5. Set the **Protocol** to HTTP and **Port** to 80.  
6. Register the `devops-ec2` instance as a target.  
7. Click **Create Target Group**.  

### Via CLI  
```bash  
aws elbv2 create-target-group --name devops-tg --protocol HTTP --port 80 --vpc-id <vpc-id>  
aws elbv2 register-targets --target-group-arn <target-group-arn> --targets Id=<instance-id>  
```  

---

## 5. Route Traffic  

### Via Console  
1. Navigate to the **Listeners** tab of the `devops-alb`.  
2. Click **Add Rule** under the HTTP listener.  
3. Forward traffic to the `devops-tg` target group.  

### Via CLI  
```bash  
aws elbv2 create-listener --load-balancer-arn <alb-arn> --protocol HTTP --port 80 --default-actions Type=forward,TargetGroupArn=<target-group-arn>  
```  

---

## 6. Security Group Adjustments  

1. Ensure the **default security group** attached to the ALB allows inbound traffic on port 80 from `0.0.0.0/0`.  
2. Ensure the **devops-sg** allows traffic from the ALB's default security group.  

### Via CLI  
```bash  
aws ec2 authorize-security-group-ingress --group-id <default-security-group-id> --protocol tcp --port 80 --cidr 0.0.0.0/0  
```  

---

## 7. Verify  

1. Retrieve the ALB DNS name from the **Load Balancer** section.  
2. Open the DNS name in a browser to verify the Nginx server is accessible.  
