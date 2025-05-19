# AWS RDS and EC2 Setup Handbook  

This handbook provides step-by-step instructions to create a private RDS instance, configure an existing EC2 instance, and establish a connection between them.  

---

## 1. Create a Private RDS Instance  

### Specifications:  
- **Instance Name**: `ops-rds`  
- **Engine Type**: MySQL v8.0.36  
- **Instance Class**: `db.t3.micro`  
- **Master Username**: `ops_admin`  
- **Password**: Choose a secure password  
- **Storage Type**: `gp2`  
- **Storage Size**: 5 GiB  
- **Database Name**: `ops_db`  
- **Other Configurations**: Default  

### Steps:  
1. Navigate to the **RDS Console** in AWS.  
2. Click on **Create database** and select the **Free Tier** template.  
3. Configure the database with the specifications mentioned above.  
4. Launch the RDS instance and wait until its status changes to **Available**.  

---

## 2. Configure Security Groups  

1. Open the **Security Groups** section in the AWS Management Console.  
2. Modify the security group associated with the RDS instance:  
    - Allow inbound traffic on **port 3306** from the **ops-ec2** instance.  
3. Modify the security group associated with the EC2 instance:  
    - Allow inbound traffic on **port 80** from the public.  

---

## 3. Configure the EC2 Instance  

### Steps:  
1. Connect to the **ops-ec2** instance from the AWS Console.  
2. On the **aws-client host**, check if an SSH key exists at `/root/.ssh/id_rsa`. If not, generate one:  
    ```bash
    ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa -N ""
    ```  
3. Copy the public key to the **authorized_keys** file of the root user on the EC2 instance:  
    ```bash
    ssh-copy-id -i /root/.ssh/id_rsa.pub root@<ops-ec2-public-ip>
    ```  

---

## 4. Deploy the PHP Application  

1. On the **aws-client host**, locate the `index.php` file under `/root`.  
2. Copy the file to the EC2 instance:  
    ```bash
    scp /root/index.php root@<ops-ec2-public-ip>:/var/www/html/
    ```  
3. Edit the `index.php` file on the EC2 instance to include the RDS connection details:  
    ```php
    <?php
    $servername = "ops-rds.<region>.rds.amazonaws.com";
    $username = "ops_admin";
    $password = "your-rds-password";
    $dbname = "ops_db";

    // Create connection
    $conn = new mysqli($servername, $username, $password, $dbname);

    // Check connection
    if ($conn->connect_error) {
         die("Connection failed: " . $conn->connect_error);
    }
    echo "Connected successfully";
    ?>
    ```  

---

## 5. Validate the Setup  

1. Access the EC2 instance using its **public IP** in a browser (e.g., `http://<ops-ec2-public-ip>`).  
2. Verify that the page displays the message:  
    ```
    Connected successfully
    ```  

---

## Notes  

- Replace `<region>` in the `$servername` variable with the appropriate AWS region of your RDS instance.  
- Ensure the RDS instance is in the **Available** state before testing the connection.  
- Use secure practices for managing passwords and keys.  

---  