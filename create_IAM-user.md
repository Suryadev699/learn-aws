# Creating an AWS IAM User, Group, Policy, and Role

## Prerequisites
- An AWS account with administrative access.
- AWS CLI installed and configured with administrative credentials.

## Method 1: Using the AWS Management Console

### Step 1: Create an IAM User
1. Sign in to the AWS Management Console.
2. Navigate to the IAM service.
3. In the left navigation pane, click on "Users" and then "Add user".
4. Enter a username and select the type of access (Programmatic access, AWS Management Console access, or both).
5. Click "Next: Permissions".

### Step 2: Create an IAM Group
1. On the "Set permissions" page, click "Create group".
2. Enter a group name and select the policies to attach to the group.
3. Click "Create group".
4. Select the newly created group and click "Next: Tags".

### Step 3: Attach Policy to the User
1. Optionally add tags to the user.
2. Review the user details and click "Create user".

### Step 4: Create an IAM Role
1. In the left navigation pane, click on "Roles" and then "Create role".
2. Select the type of trusted entity (e.g., AWS service, Another AWS account).
3. Choose the service that will use this role.
4. Attach the necessary policies.
5. Optionally add tags and review the role details.
6. Click "Create role".

## Method 2: Using the AWS CLI

### Step 1: Create an IAM User
```sh
aws iam create-user --user-name NewUser
```

### Step 2: Create an IAM Group
```sh
aws iam create-group --group-name NewGroup
```

### Step 3: Attach Policy to the Group
```sh
aws iam attach-group-policy --group-name NewGroup --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### Step 4: Add User to the Group
```sh
aws iam add-user-to-group --user-name NewUser --group-name NewGroup
```

### Step 5: Create an IAM Role
```sh
aws iam create-role --role-name NewRole --assume-role-policy-document file://trust-policy.json
```

### Step 6: Attach Policy to the Role
```sh
aws iam attach-role-policy --role-name NewRole --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

### Example `trust-policy.json`
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
