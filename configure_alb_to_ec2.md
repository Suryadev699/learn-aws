# Setting Up an Application Load Balancer (ALB) for an EC2 Instance

## Prerequisites
- An EC2 instance running in a public subnet.
- A security group for the EC2 instance allowing traffic on the desired port (e.g., 80).
- IAM permissions to create and manage ALBs.

---

## Method 1: Using AWS Management Console

### Step 1: Create a Target Group
1. Navigate to the **EC2 Dashboard**.
2. Under **Load Balancing**, select **Target Groups**.
3. Click **Create target group**.
4. Choose **Instances** as the target type.
5. Configure the target group name, protocol (e.g., HTTP), and port (e.g., 80).
6. Select the VPC where your EC2 instance resides.
7. Click **Next** and register your EC2 instance to the target group.
8. Click **Create target group**.

### Step 2: Create an Application Load Balancer
1. Navigate to **Load Balancers** under **Load Balancing**.
2. Click **Create Load Balancer** and select **Application Load Balancer**.
3. Configure the ALB name, scheme (Internet-facing or Internal), and IP address type.
4. Select the VPC and at least two public subnets.
5. Configure the security group to allow HTTP/HTTPS traffic.
6. Under **Listeners**, set the protocol and port (e.g., HTTP:80).
7. Choose the target group created earlier.
8. Click **Create Load Balancer**.

### Step 3: Test the ALB
1. Copy the DNS name of the ALB from the **Load Balancer** details.
2. Open the DNS name in a browser to verify traffic is routed to the EC2 instance.

---

## Method 2: Using AWS CLI

### Step 1: Create a Target Group
```bash
aws elbv2 create-target-group \
    --name my-target-group \
    --protocol HTTP \
    --port 80 \
    --vpc-id <vpc-id> \
    --target-type instance
```

### Step 2: Register Targets
```bash
aws elbv2 register-targets \
    --target-group-arn <target-group-arn> \
    --targets Id=<instance-id>
```

### Step 3: Create an ALB
```bash
aws elbv2 create-load-balancer \
    --name my-alb \
    --subnets <subnet-1-id> <subnet-2-id> \
    --security-groups <security-group-id> \
    --scheme internet-facing \
    --type application \
    --ip-address-type ipv4
```

### Step 4: Create a Listener
```bash
aws elbv2 create-listener \
    --load-balancer-arn <load-balancer-arn> \
    --protocol HTTP \
    --port 80 \
    --default-actions Type=forward,TargetGroupArn=<target-group-arn>
```

### Step 5: Test the ALB
1. Retrieve the DNS name of the ALB:
   ```bash
   aws elbv2 describe-load-balancers --names my-alb
   ```
2. Open the DNS name in a browser to verify traffic is routed to the EC2 instance.

---

## Notes
- Replace placeholders (e.g., `<vpc-id>`, `<instance-id>`) with actual values.
- Ensure proper security group rules are configured for both the ALB and EC2 instance.
