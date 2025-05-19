# Handbook: Hosting a Static Website on AWS S3

This handbook provides step-by-step instructions for hosting a static website on AWS using an S3 bucket. The website will be publicly accessible via the S3 website URL.

---

## Prerequisites
- AWS account with necessary permissions to create and manage S3 buckets.
- `index.html` file located in the `/root/` directory of the AWS client host.

---

## Steps to Host a Static Website on S3

### 1. Create an S3 Bucket
1. Log in to the AWS Management Console.
2. Navigate to the **S3** service.
3. Click **Create bucket**.
4. Provide a unique bucket name (e.g., `my-static-website-bucket`).
5. Choose the appropriate AWS Region.
6. Uncheck the **Block all public access** option to allow public access.
7. Acknowledge the warning and click **Create bucket**.

---

### 2. Configure the S3 Bucket for Static Website Hosting
1. Open the newly created bucket.
2. Go to the **Properties** tab.
3. Scroll down to the **Static website hosting** section.
4. Enable static website hosting.
5. Set the **Index document** to `index.html`.
6. Save the changes.

---

### 3. Allow Public Access to the Bucket
1. Go to the **Permissions** tab of the bucket.
2. Under **Bucket Policy**, click **Edit**.
3. Add the following bucket policy to allow public read access:
    ```json
    {
         "Version": "2012-10-17",
         "Statement": [
              {
                    "Effect": "Allow",
                    "Principal": "*",
                    "Action": "s3:GetObject",
                    "Resource": "arn:aws:s3:::my-static-website-bucket/*"
              }
         ]
    }
    ```
4. Replace `my-static-website-bucket` with your bucket name.
5. Save the policy.

---

### 4. Upload the `index.html` File
1. Open the bucket in the S3 console.
2. Click **Upload**.
3. Select the `index.html` file from the `/root/` directory of your AWS client host.
4. Click **Upload** to complete the process.

---

### 5. Verify Website Accessibility
1. Go to the **Properties** tab of the bucket.
2. In the **Static website hosting** section, note the **Endpoint** URL.
3. Open the URL in a web browser to verify that the website is accessible.

---

## Troubleshooting
- Ensure the bucket policy allows public access.
- Verify that the `index.html` file is uploaded to the root of the bucket.
- Check the **Static website hosting** configuration for errors.

---

You have successfully hosted a static website on AWS S3!  