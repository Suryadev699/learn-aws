# Creating, Modifying, and Terminating an EC2 Instance via AWS CLI

This guide provides step-by-step instructions to create, modify, and terminate an EC2 instance using the AWS CLI.

---

## Prerequisites
1. **Install AWS CLI**: Ensure the AWS CLI is installed on your system. [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
2. **Configure AWS CLI**: Run `aws configure` to set up your credentials and default region.
3. **IAM Permissions**: Ensure your IAM user/role has permissions for EC2 operations.

---

## Step 1: Create an EC2 Instance
1. **Find an AMI**:
    ```bash
    aws ec2 describe-images --owners amazon --query 'Images[*].[ImageId,Name]' --output table
    ```
    Note the `ImageId` of the desired AMI.

2. **Create a Key Pair** (if not already created):
    ```bash
    aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
    chmod 400 MyKeyPair.pem
    ```

3. **Launch the EC2 Instance**:
    ```bash
    aws ec2 run-instances \
         --image-id ami-xxxxxxxxxxxxxxxxx \
         --count 1 \
         --instance-type t2.micro \
         --key-name MyKeyPair \
         --security-group-ids sg-xxxxxxxx \
         --subnet-id subnet-xxxxxxxx
    ```
    Replace placeholders with appropriate values.

4. **Verify the Instance**:
    ```bash
    aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]' --output table
    ```

---

## Step 2: Modify the EC2 Instance
1. **Stop the Instance**:
    ```bash
    aws ec2 stop-instances --instance-ids i-xxxxxxxxxxxxxxxxx
    ```

2. **Change the Instance Type**:
    ```bash
    aws ec2 modify-instance-attribute --instance-id i-xxxxxxxxxxxxxxxxx --instance-type "{\"Value\": \"t2.small\"}"
    ```

3. **Change the AMI**:
    - Create a new instance from the desired AMI.
    - Migrate data from the old instance (if needed).
    - Terminate the old instance (see Step 3).

4. **Start the Instance**:
    ```bash
    aws ec2 start-instances --instance-ids i-xxxxxxxxxxxxxxxxx
    ```

---

## Step 3: Terminate the EC2 Instance
1. **Terminate the Instance**:
    ```bash
    aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxxxxxx
    ```

2. **Verify Termination**:
    ```bash
    aws ec2 describe-instances --instance-ids i-xxxxxxxxxxxxxxxxx --query 'Reservations[*].Instances[*].State.Name' --output text
    ```

---

## Notes
- Replace `i-xxxxxxxxxxxxxxxxx` with your instance ID.
- Ensure you clean up resources (e.g., key pairs, security groups) to avoid unnecessary charges.
