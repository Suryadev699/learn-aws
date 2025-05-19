# Setting Up a CloudWatch Alarm for an EC2 Instance

Follow these steps to set up a CloudWatch alarm for an EC2 instance, create an SNS topic, and test the alarm using the `stress` package.

## Step 1: Create an SNS Topic
1. Open the **AWS Management Console** and navigate to the **SNS** service.
2. Click on **Topics** in the left-hand menu.
3. Click **Create topic**.
    - **Type**: Standard.
    - **Name**: Provide a name (e.g., `EC2-Alarm-Topic`).
4. Click **Create topic**.
5. After creating the topic, click on it and choose **Create subscription**.
    - **Protocol**: Email.
    - **Endpoint**: Enter your email address.
6. Confirm the subscription by clicking the link in the email you receive.

## Step 2: Create a CloudWatch Alarm
1. Navigate to the **CloudWatch** service in the AWS Management Console.
2. In the left-hand menu, click **Alarms** and then **Create alarm**.
3. Click **Select metric**.
    - Choose **EC2** > **Per-Instance Metrics**.
    - Select the instance you want to monitor and choose a metric (e.g., `CPUUtilization`).
4. Set the alarm conditions:
    - **Threshold type**: Static.
    - **Whenever CPUUtilization is...**: Greater than 80% for 2 consecutive periods of 1 minute.
5. Configure actions:
    - **Notification**: Select the SNS topic created earlier.
6. Provide a name for the alarm (e.g., `High-CPU-Alarm`) and click **Create alarm**.

## Step 3: Install the `stress` Package on the EC2 Instance
1. Connect to your EC2 instance via SSH.
2. Install the `stress` package:
    ```bash
    sudo apt update && sudo apt install stress -y  # For Ubuntu/Debian
    sudo yum install stress -y                    # For Amazon Linux/CentOS
    ```
3. Run the `stress` command to simulate high CPU usage:
    ```bash
    stress --cpu 4 --timeout 300
    ```
    - This command will utilize 4 CPU cores for 5 minutes.

## Step 4: Verify the Alarm
1. Wait for the CloudWatch alarm to trigger based on the defined threshold.
2. Check your email for the SNS notification.
3. Once the alarm is triggered, stop the `stress` process:
    ```bash
    pkill stress
    ```

## Step 5: Clean Up Resources
1. Delete the CloudWatch alarm.
2. Unsubscribe from the SNS topic and delete it if no longer needed.
