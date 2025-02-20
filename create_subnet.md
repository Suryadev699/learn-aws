# Creating an AWS Subnet

## Prerequisites
- An AWS account
- Appropriate IAM permissions to create subnets
- An existing VPC (Virtual Private Cloud)

## Method 1: Creating a Subnet via the AWS Management Console

1. Sign in to the [AWS Management Console](https://aws.amazon.com/console/).
2. In the top navigation bar, select the region where you want to create the subnet.
3. Navigate to the VPC Dashboard by selecting **Services** > **VPC**.
4. In the left-hand navigation pane, select **Subnets**.
5. Click the **Create subnet** button.
6. In the **Create subnet** page, provide the following details:
    - **Name tag**: (Optional) A name for your subnet.
    - **VPC**: Select the VPC in which you want to create the subnet.
    - **Availability Zone**: Select an Availability Zone.
    - **IPv4 CIDR block**: Enter the IPv4 CIDR block for the subnet (e.g., 10.0.1.0/24).
    - (Optional) **IPv6 CIDR block**: Enter the IPv6 CIDR block if needed.
7. Click the **Create subnet** button.

## Method 2: Creating a Subnet via the AWS CLI

1. Open your terminal or command prompt.
2. Ensure that the AWS CLI is installed and configured with the necessary credentials.
3. Use the following command to create a subnet:

```sh
aws ec2 create-subnet \
     --vpc-id vpc-xxxxxxxx \
     --cidr-block 10.0.1.0/24 \
     --availability-zone us-west-2a \
     --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=MySubnet}]'
```

Replace `vpc-xxxxxxxx` with your VPC ID, `10.0.1.0/24` with your desired CIDR block, `us-west-2a` with your desired Availability Zone, and `MySubnet` with your desired subnet name.

4. Verify the creation of the subnet by running:

```sh
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-xxxxxxxx"
```

Replace `vpc-xxxxxxxx` with your VPC ID.
