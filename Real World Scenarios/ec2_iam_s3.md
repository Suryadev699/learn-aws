# EC2, IAM, and S3 Setup Handbook  

## Task Overview  
This handbook provides step-by-step instructions to set up a new EC2 instance, configure SSH keys, create a private S3 bucket, and assign IAM roles and policies for secure access.  

---

## 1. EC2 Instance Setup  

### Steps to Create a New EC2 Instance  

1. **Launch a New EC2 Instance**:  
    Use the AWS Management Console or CLI to launch a new EC2 instance.  

2. **Configure Security Group**:  
    Ensure the security group allows SSH access (port 22) from your IP address.  

3. **Note the Instance ID and Public IP**:  
    Record the instance ID and public IP for future steps.  

---

## 2. Setup SSH Keys  

### Steps to Create and Configure SSH Keys  

1. **Generate SSH Key Pair on AWS Client Host**:  
    Run the following command on the AWS client host:  
    ```bash  
    ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa  
    ```  
    This will generate `id_rsa` (private key) and `id_rsa.pub` (public key).  

2. **Add Public Key to EC2 Instance**:  
    Copy the public key to the EC2 instance:  
    ```bash  
    ssh-copy-id -i ~/.ssh/id_rsa.pub root@<EC2-Instance-IP>  
    ```  

3. **Verify SSH Access**:  
    Test SSH access to the EC2 instance:  
    ```bash  
    ssh -i ~/.ssh/id_rsa root@<EC2-Instance-IP>  
    ```  

---

## 3. Create a Private S3 Bucket  

### Steps to Create the Bucket  

1. **Create the Bucket**:  
    Use the AWS Management Console or CLI to create a private S3 bucket.  
    ```bash  
    aws s3api create-bucket --bucket <bucket-name> --region <region>  
    ```  

2. **Ensure Bucket Privacy**:  
    Block public access to the bucket:  
    ```bash  
    aws s3api put-public-access-block --bucket <bucket-name> --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true  
    ```  

---

## 4. Create IAM Policy and Role  

### Steps to Create IAM Policy  

1. **Define the Policy**:  
    Create a JSON file `<policy-name>.json` with the following content:  
    ```json  
    {  
         "Version": "2012-10-17",  
         "Statement": [  
              {  
                    "Effect": "Allow",  
                    "Action": [  
                         "s3:PutObject",  
                         "s3:ListBucket",  
                         "s3:GetObject"  
                    ],  
                    "Resource": [  
                         "arn:aws:s3:::<bucket-name>",  
                         "arn:aws:s3:::<bucket-name>/*"  
                    ]  
              }  
         ]  
    }  
    ```  

2. **Create the Policy**:  
    Use the AWS CLI to create the policy:  
    ```bash  
    aws iam create-policy --policy-name <policy-name> --policy-document file://<policy-name>.json  
    ```  

### Steps to Create IAM Role  

1. **Create the Role**:  
    Create an IAM role with EC2 as the trusted entity:  
    ```bash  
    aws iam create-role --role-name <role-name> --assume-role-policy-document file://trust-policy.json  
    ```  
    Example `trust-policy.json`:  
    ```json  
    {  
         "Version": "2012-10-17",  
         "Statement": [  
              {  
                    "Effect": "Allow",  
                    "Principal": {  
                         "Service": "ec2.amazonaws.com"  
                    },  
                    "Action": "sts:AssumeRole"  
              }  
         ]  
    }  
    ```  

2. **Attach the Policy to the Role**:  
    ```bash  
    aws iam attach-role-policy --role-name <role-name> --policy-arn arn:aws:iam::<account-id>:policy/<policy-name>  
    ```  

3. **Attach the Role to EC2 Instance**:  
    ```bash  
    aws ec2 associate-iam-instance-profile --instance-id <instance-id> --iam-instance-profile Name=<role-name>  
    ```  

---

## 5. Test the Access  

### Steps to Test  

1. **SSH into the EC2 Instance**:  
    ```bash  
    ssh -i ~/.ssh/id_rsa root@<EC2-Instance-IP>  
    ```  

2. **Upload a File to the S3 Bucket**:  
    ```bash  
    echo "Test File" > test-file.txt  
    aws s3 cp test-file.txt s3://<bucket-name>/  
    ```  

3. **List the Uploaded File**:  
    ```bash  
    aws s3 ls s3://<bucket-name>/  
    ```  

If the file is listed successfully, the setup is complete.  

---  