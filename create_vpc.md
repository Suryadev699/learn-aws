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


## Configuring a Public VPC with an EC2 for Internet Access

### Step 1: Create a Public Subnet
1. Navigate to the **Subnets** section in the VPC Dashboard.
2. Click on **Create Subnet**.
3. Select your VPC and specify a name for the subnet.
4. Define a CIDR block (e.g., `10.0.1.0/24`).
5. Click **Create Subnet**.

### Step 2: Enable Auto-Assign Public IP
1. Select the newly created subnet.
2. Click on the **Actions** dropdown and choose **Modify auto-assign IP settings**.
3. Check the box for **Enable auto-assign public IPv4 address**.
4. Save the changes.

### Step 3: Create an Internet Gateway
1. Navigate to the **Internet Gateways** section in the VPC Dashboard.
2. Click on **Create Internet Gateway**.
3. Provide a name for the Internet Gateway and click **Create**.
4. Attach the Internet Gateway to your VPC by selecting it and clicking **Actions** > **Attach to VPC**.

### Step 4: Update Route Table
1. Navigate to the **Route Tables** section in the VPC Dashboard.
2. Select the route table associated with your public subnet.
3. Click on the **Routes** tab and then **Edit routes**.
4. Add a new route with:
    - **Destination**: `0.0.0.0/0`
    - **Target**: The Internet Gateway you created.
5. Save the changes.

### Step 5: Launch an EC2 Instance
1. Navigate to the **EC2 Dashboard** and click **Launch Instance**.
2. Select an Amazon Machine Image (AMI) and instance type.
3. Place the instance in the public subnet and ensure **Auto-assign Public IP** is enabled.
4. Configure security groups to allow SSH (port 22) and any other required ports.
5. Launch the instance and connect using the public IP.

---

## Establishing a Secure Connection Between Public and Private VPC via VPC Peering

### Step 1: Create a Private VPC
1. Follow the steps in **Method 1** or **Method 2** to create another VPC with a different CIDR block (e.g., `10.1.0.0/16`).

### Step 2: Create a VPC Peering Connection
1. Navigate to the **VPC Peering Connections** section in the VPC Dashboard.
2. Click on **Create Peering Connection**.
3. Specify:
    - **Requester VPC**: Your public VPC.
    - **Accepter VPC**: Your private VPC.
4. Click **Create Peering Connection**.

### Step 3: Accept the Peering Connection
1. Select the newly created peering connection.
2. Click **Actions** > **Accept Request**.

### Step 4: Update Route Tables
1. Navigate to the **Route Tables** section.
2. For the public VPC's route table:
    - Add a route with:
      - **Destination**: The CIDR block of the private VPC (e.g., `10.1.0.0/16`).
      - **Target**: The VPC Peering Connection.
3. For the private VPC's route table:
    - Add a route with:
      - **Destination**: The CIDR block of the public VPC (e.g., `10.0.0.0/16`).
      - **Target**: The VPC Peering Connection.

### Step 5: Test the Connection
1. Launch an EC2 instance in the private VPC.
2. Ensure the security groups and network ACLs allow traffic between the two VPCs.
3. Verify connectivity by pinging or SSHing between instances in the public and private VPCs.


## Configuring a Private VPC with an EC2 Instance

### Step 1: Create a Private Subnet
1. Navigate to the **Subnets** section in the VPC Dashboard.
2. Click on **Create Subnet**.
3. Select your VPC and specify a name for the subnet (e.g., "Private Subnet").
4. Define a CIDR block (e.g., `10.1.1.0/24`).
5. Click **Create Subnet**.

### Step 2: Disable Auto-Assign Public IP
1. Select the newly created subnet.
2. Click on the **Actions** dropdown and choose **Modify auto-assign IP settings**.
3. Ensure the box for **Enable auto-assign public IPv4 address** is unchecked.
4. Save the changes.

### Step 3: Launch an EC2 Instance
1. Navigate to the **EC2 Dashboard** and click **Launch Instance**.
2. Select an Amazon Machine Image (AMI) and instance type.
3. Place the instance in the private subnet and ensure **Auto-assign Public IP** is disabled.
4. Configure security groups to allow necessary traffic (e.g., SSH from a bastion host or specific IPs).
5. Launch the instance.

### Step 4: Access the Private EC2 Instance
1. Use a bastion host or VPN connection to securely access the private EC2 instance.
2. Ensure the security groups and network ACLs are configured to allow traffic from the bastion host or VPN.
