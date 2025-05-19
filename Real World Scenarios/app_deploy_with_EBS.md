# Handbook: Deploying a Web Application with AWS Elastic Beanstalk

This handbook provides step-by-step instructions to deploy a Python web application using AWS Elastic Beanstalk.

---

## 1. Create an Elastic Beanstalk Application
1. Open the [AWS Management Console](https://aws.amazon.com/console/).
2. Navigate to **Elastic Beanstalk**.
3. Click **Create Application**.
4. Provide the following details:
    - **Application Name**: `webapp`
5. Click **Create**.

---

## 2. Create an Elastic Beanstalk Environment
1. In the Elastic Beanstalk console, select the `webapp` application.
2. Click **Create Environment**.
3. Choose **Web server environment**.
4. Provide the following details:
    - **Environment Name**: `webapp-env`
    - **Platform**: `Python`
    - **Environment Type**: `Single instance environment`
5. Click **Create Environment**.

---

## 3. Create a Private S3 Bucket
1. Navigate to the **S3** service in the AWS Management Console.
2. Click **Create bucket**.
3. Provide a unique **Bucket Name**.
4. Ensure **Block all public access** is enabled.
5. Click **Create bucket**.

---

## 4. Upload the Application Code to S3
1. On the client host, locate the application code at `/root/app.zip`.
2. Use the AWS CLI to upload the file to the S3 bucket:
    ```bash
    aws s3 cp /root/app.zip s3://<your-bucket-name>/
    ```
    Replace `<your-bucket-name>` with the name of your S3 bucket.

---

## 5. Create the IAM Role
1. Navigate to the **IAM** service in the AWS Management Console.
2. Click **Roles** > **Create role**.
3. Select **AWS service** and choose **Elastic Beanstalk**.
4. Attach the policy **AWSElasticBeanstalkWebTier**.
5. Name the role `aws-elasticbeanstalk-ec2-role`.
6. Click **Create role**.

---

## 6. Deploy the Application
1. In the Elastic Beanstalk console, select the `webapp-env` environment.
2. Click **Upload and Deploy**.
3. Choose **S3** as the source and provide the path to the uploaded `app.zip` file.
4. Click **Deploy**.

---

## 7. Verify the Deployment
1. Wait for the deployment to complete.
2. Access the application using the environment's URL provided in the Elastic Beanstalk console.
3. Ensure the application is running successfully.

---

**Congratulations!** You have successfully deployed your Python web application using AWS Elastic Beanstalk.  