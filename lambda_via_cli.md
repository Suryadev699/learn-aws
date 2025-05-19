# Creating an AWS Lambda Function via CLI: Step-by-Step Guide  

Follow this guide to create a Lambda function using the AWS CLI. We'll use an example scenario where the Lambda function returns "Hello World!" with a status code of 200.  

---

## 1. **Create the Python Script**  
Write a Python script named `lambda_function.py` with the following content:  

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello World!'
    }
```

---

## 2. **Zip the Python Script**  
Compress the script into a `.zip` file:  

```bash
zip function.zip lambda_function.py
```

---

## 3. **Create an IAM Role**  
Create an IAM role with the necessary permissions for Lambda.  

1. **Trust Policy**: Save the following trust policy to a file named `trust-policy.json`:  
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "lambda.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }
    ```

2. **Create the Role**:  
    ```bash
    aws iam create-role --role-name lambda-cli-role --assume-role-policy-document file://trust-policy.json
    ```

3. **Attach Policy**: Attach the AWS-managed policy for basic Lambda execution:  
    ```bash
    aws iam attach-role-policy --role-name lambda-cli-role --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
    ```

---

## 4. **Create the Lambda Function**  
Use the zipped file and IAM role to create the Lambda function:  

1. **Get the Role ARN**:  
    ```bash
    aws iam get-role --role-name lambda-cli-role --query 'Role.Arn' --output text
    ```

2. **Create the Function**: Replace `<ROLE_ARN>` with the ARN from the previous step:  
    ```bash
    aws lambda create-function \
        --function-name ops-lambda-cli \
        --runtime python3.x \
        --role <ROLE_ARN> \
        --handler lambda_function.lambda_handler \
        --zip-file fileb://function.zip
    ```

---

## 5. **Test the Lambda Function**  
Invoke the Lambda function to ensure it works as expected:  

```bash
aws lambda invoke \
    --function-name ops-lambda-cli \
    --payload '{}' \
    response.json
```

Check the `response.json` file for the output. It should contain:  
```json
{
    "statusCode": 200,
    "body": "Hello World!"
}
```

---

You're all set! 🎉 You've successfully created and tested a Lambda function using the AWS CLI.  