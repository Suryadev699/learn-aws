# Creating an AWS VPC

## Prerequisites
- An AWS account with appropriate permissions to create VPCs.
- AWS CLI installed and configured with your credentials.

## Method 1: Creating a VPC via the AWS Management Console

### Step 1: Define VPC CIDR Block
1. Sign in to the AWS Management Console.
2. Open the VPC Dashboard by navigating to **Services** > **VPC**.
3. Click on **Your VPCs** in the left-hand menu.
4. Click on the **Create VPC** button.
5. Enter a name for your VPC.
6. Specify an IPv4 CIDR block (e.g., `10.0.0.0/16`).

### Step 2: Create an IPv4 VPC
1. Ensure the **IPv4 CIDR block** is filled in.
2. Leave the **IPv6 CIDR block** as "No IPv6 CIDR Block".
3. Click on **Create VPC**.

### Step 3: Create an IPv6 VPC
1. Repeat steps 1-4 from above.
2. Specify an IPv4 CIDR block (e.g., `10.0.0.0/16`).
3. For **IPv6 CIDR block**, select "Amazon provided IPv6 CIDR block".
4. Click on **Create VPC**.

## Method 2: Creating a VPC via the AWS CLI

### Step 1: Define VPC CIDR Block
1. Open your terminal or command prompt.
2. Ensure AWS CLI is installed and configured.

### Step 2: Create an IPv4 VPC
```sh
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

### Step 3: Create an IPv6 VPC
```sh
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --amazon-provided-ipv6-cidr-block
```

### Step 4: Verify VPC Creation
```sh
aws ec2 describe-vpcs
```

This command will list all VPCs in your account, including the newly created ones.
