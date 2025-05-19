# Handbook: Implementing a Lambda Function Using CloudFormation  

## Objective  
This guide helps you create a Lambda function using a CloudFormation stack. The stack name will be `ops-lambda-app`.  

## Steps  

### 1. Create the IAM Role  
1. Log in to the AWS Management Console.  
2. Navigate to the **IAM** service.  
3. Create a role named `lambda_execution_role` with the following:  
    - **Trusted entity**: AWS Lambda.  
    - **Permissions**: Attach the `AWSLambdaBasicExecutionRole` policy.  
4. Note the ARN of the created role (e.g., `arn:aws:iam::123456789012:role/lambda_execution_role`).  

### 2. Create the CloudFormation Template  
1. On the host, create a file named `/root/ops-lambda.yml`.  
2. Add the following content to the file:  

```yaml  
AWSTemplateFormatVersion: '2010-09-09'  
Description: ops-lambda-app  

Resources:  

  OpsLambda:  
     Type: AWS::Lambda::Function  
     Properties:  
        FunctionName: ops-lambda  
        Runtime: python3.9  
        Handler: index.lambda_handler  
        Role: arn:aws:iam::123456789012:role/lambda_execution_role # Replace with the ARN of the IAM role  
        Code:  
          ZipFile: |  
             import json  

             def lambda_handler(event, context):  
                  return {  
                        'statusCode': 200,  
                        'body': 'Welcome to KKE AWS Labs!'  
                  }  

Outputs:  
  LambdaFunctionName:  
     Description: Name of the created Lambda function  
     Value: !Ref OpsLambda  
```  

### 3. Validate the Template  
Run the following command to validate the template:  
```bash  
aws cloudformation validate-template --template-body file:///root/ops-lambda.yml  
```  

### 4. Deploy the CloudFormation Stack  
Run the following command to create the stack:  
```bash  
aws cloudformation create-stack --stack-name ops-lambda-app --template-body file:///root/ops-lambda.yml  
```  

### 5. Verify the Deployment  
1. Navigate to the **CloudFormation** service in the AWS Management Console.  
2. Confirm that the stack `ops-lambda-app` is created successfully.  
3. Navigate to the **Lambda** service and verify the function `ops-lambda` exists.  

### 6. Test the Lambda Function  
1. Open the `ops-lambda` function in the AWS Lambda console.  
2. Create a test event and invoke the function.  
3. Confirm the response:  
    ```json  
    {  
         "statusCode": 200,  
         "body": "Welcome to KKE AWS Labs!"  
    }  
    ```  

## Notes  
- Replace the placeholder ARN in the template with the actual ARN of the IAM role.  
- Ensure the AWS CLI is configured with appropriate permissions to create resources.  
