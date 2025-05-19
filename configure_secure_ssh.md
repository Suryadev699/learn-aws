# Configuring Secure SSH for an EC2 Instance

## Prerequisites
- An AWS EC2 instance running.
- SSH client installed on your local machine.
- Key pair for the EC2 instance.

---

## Step 1: Connect to Your EC2 Instance
1. Open your terminal.
2. Use the following command to connect to your EC2 instance:
    ```bash
    ssh -i /path/to/your-key.pem ec2-user@<EC2-Public-IP>
    ```

---

## Step 2: Update the System
1. Once connected, update the system packages:
    ```bash
    sudo yum update -y
    ```

---

## Step 3: Configure SSH Daemon for Security
1. Open the SSH configuration file:
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
2. Modify the following settings:
    - Disable root login:
      ```plaintext
      PermitRootLogin no
      ```
    - Disable password authentication:
      ```plaintext
      PasswordAuthentication no
      ```
    - Set a custom SSH port (optional):
      ```plaintext
      Port 2222
      ```
3. Save and exit the file.

---

## Step 4: Restart the SSH Service
1. Restart the SSH service to apply changes:
    ```bash
    sudo systemctl restart sshd
    ```

---

## Step 5: Update Security Group Rules
1. In the AWS Management Console, navigate to the EC2 instance's security group.
2. Add an inbound rule for the custom SSH port (if changed) and restrict access to your IP address.

---

# Creating Passwordless SSH

## Step 1: Generate SSH Key Pair on Local Machine
1. Run the following command on your local machine:
    ```bash
    ssh-keygen -t rsa -b 4096
    ```
2. Save the key pair in the default location (e.g., `~/.ssh/id_rsa`).

---

## Step 2: Copy Public Key to EC2 Instance
1. Use the `ssh-copy-id` command:
    ```bash
    ssh-copy-id -i ~/.ssh/id_rsa.pub ec2-user@<EC2-Public-IP>
    ```
    If `ssh-copy-id` is unavailable, manually copy the key:
    ```bash
    cat ~/.ssh/id_rsa.pub | ssh -i /path/to/your-key.pem ec2-user@<EC2-Public-IP> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
    ```

---

## Step 3: Test Passwordless SSH
1. Disconnect from the EC2 instance.
2. Reconnect using:
    ```bash
    ssh ec2-user@<EC2-Public-IP>
    ```
    You should not be prompted for a password.

---

## Step 4: Secure the Private Key
1. Ensure the private key has restricted permissions:
    ```bash
    chmod 600 ~/.ssh/id_rsa
    ```

---

You have now configured secure SSH and passwordless authentication for your EC2 instance.  