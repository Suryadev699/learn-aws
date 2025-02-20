# Creating an AWS Key Pair

## Prerequisites
- An AWS account
- AWS CLI installed and configured (for CLI method)

## Method 1: Using the AWS Management Console

1. Sign in to the AWS Management Console.
2. Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
3. In the navigation pane, under "Network & Security," choose "Key Pairs."
4. Choose "Create key pair."
5. For "Key pair name," enter a descriptive name for the key pair.
6. For "Key pair type," choose either RSA or ED25519.
7. For "Private key file format," choose the format you want (PEM or PPK).
8. Choose "Create key pair."
9. The private key file is automatically downloaded by your browser. Save the private key file in a secure and accessible location.

## Method 2: Using the AWS CLI

1. Open a terminal.
2. Run the following command to create a key pair:

    ```sh
    aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
    ```

    Replace `MyKeyPair` with your desired key pair name.

3. Set the correct permissions for the private key file:

    ```sh
    chmod 400 MyKeyPair.pem
    ```

4. The key pair is now created and the private key is saved in the file `MyKeyPair.pem`.

## Notes
- Keep your private key file secure. Do not share it with anyone.
- You will need the private key file to connect to your EC2 instances.