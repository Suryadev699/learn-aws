# Setting Up AWS CodeCommit  


## Step 1: Create a CodeCommit Repository  
1. Log in to the AWS Management Console.  
2. Navigate to **CodeCommit** under the **Developer Tools** section.  
3. Click **Create Repository** and provide a name and optional description for your repository.  
4. Click **Create** to finalize.  

## Step 2: Set Up Passwordless SSH on the Client Machine  
1. Open a terminal on your client machine.  
2. Generate an SSH key pair by running:  
    ```bash  
    ssh-keygen -t rsa -b 2048  
    ```  
    - When prompted, specify a file name (e.g., `my_codecommit_key`) or press Enter to use the default.  
    - Leave the passphrase empty for passwordless access.  

3. Copy the contents of the public key file (e.g., `my_codecommit_key.pub`):  
    ```bash  
    cat ~/.ssh/my_codecommit_key.pub  
    ```  

## Step 3: Add the Public Key to IAM  
1. Go to the **IAM** section in the AWS Management Console.  
2. Select **Users** and click on your IAM user.  
3. Navigate to the **Security credentials** tab.  
4. Under **SSH keys for AWS CodeCommit**, click **Upload SSH public key**.  
5. Paste the public key you copied earlier and click **Upload SSH public key**.  
6. Note the **SSH Key ID** provided after uploading.  

## Step 4: Configure SSH on the Client Machine  
1. Navigate to the `.ssh` directory:  
    ```bash  
    cd ~/.ssh  
    ```  

2. Create or edit the `config` file:  
    ```bash  
    nano config  
    ```  

3. Add the following configuration (replace placeholders with actual values):  
    ```plaintext  
    Host git-codecommit.*.amazonaws.com  
      User <SSH Key ID from IAM>  
      IdentityFile ~/.ssh/<private_key_file_name>  
    ```  

4. Save and close the file.  

5. Set the correct permissions for the `config` file:  
    ```bash  
    chmod 600 config  
    ```  

## Step 5: Test the SSH Connection  
1. Run the following command to test the connection:  
    ```bash  
    ssh git-codecommit.us-east-1.amazonaws.com  
    ```  
    - Replace `us-east-1` with your AWS region if different.  
    - You should see a message like: `You've successfully authenticated, but CodeCommit does not provide shell access.`  

## Step 6: Clone the Repository and Start Working  
1. Clone the repository:  
    ```bash  
    git clone ssh://git-codecommit.<region>.amazonaws.com/v1/repos/<repository_name>  
    ```  

2. Navigate to the repository directory:  
    ```bash  
    cd <repository_name>  
    ```  

3. Add, commit, and push your changes as needed:  
    ```bash  
    git add .  
    git commit -m "Initial commit"  
    git push  
    ```  

You are now set up to use AWS CodeCommit with SSH!  