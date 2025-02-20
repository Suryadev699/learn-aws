# Creating an AWS Elastic IP and Linking to Resources

## Prerequisites
- An AWS account
- AWS CLI installed and configured with appropriate permissions
- Access to the AWS Management Console

## Method 1: Using the AWS Management Console

### Step 1: Allocate an Elastic IP
1. Sign in to the AWS Management Console.
2. Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
3. In the navigation pane, choose **Elastic IPs**.
4. Choose **Allocate Elastic IP address**.
5. Choose **Allocate**.

### Step 2: Associate the Elastic IP with a Resource
1. In the Elastic IPs pane, select the Elastic IP address that you allocated.
2. Choose **Actions**, then **Associate Elastic IP address**.
3. In the **Instance** or **Network interface** field, select the resource you want to associate with the Elastic IP.
4. Choose **Associate**.

## Method 2: Using the AWS CLI

### Step 1: Allocate an Elastic IP
```sh
aws ec2 allocate-address --domain vpc
```

### Step 2: Associate the Elastic IP with an Instance
```sh
aws ec2 associate-address --instance-id i-1234567890abcdef0 --allocation-id eipalloc-12345678
```

### Step 2: Associate the Elastic IP with a Network Interface
```sh
aws ec2 associate-address --network-interface-id eni-12345678 --allocation-id eipalloc-12345678
```

Replace `i-1234567890abcdef0`, `eipalloc-12345678`, and `eni-12345678` with your actual instance ID, allocation ID, and network interface ID respectively.
