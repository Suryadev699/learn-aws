# Encrypt Data Using AWS KMS

This handbook provides a step-by-step guide to encrypt and decrypt data using AWS Key Management Service (KMS).

---

## Prerequisites
1. AWS CLI installed and configured with appropriate permissions.
2. Access to the AWS Management Console or CLI.
3. A file named `SensitiveData.txt` located in `/root/`.

---

## Steps

### 1. Create a Symmetric KMS Key
1. Open the AWS Management Console or use the AWS CLI to create a KMS key.
2. Run the following command to create a symmetric KMS key named `nautilus-KMS-Key`:
    ```bash
    aws kms create-key --description "nautilus-KMS-Key" --key-usage ENCRYPT_DECRYPT --origin AWS_KMS
    ```
3. Note the `KeyId` from the output. You will use it in subsequent steps.

4. Optionally, assign an alias to the key for easier reference:
    ```bash
    aws kms create-alias --alias-name alias/nautilus-KMS-Key --target-key-id <KeyId>
    ```

---

### 2. Encrypt the File
1. Use the following command to encrypt the `SensitiveData.txt` file:
    ```bash
    aws kms encrypt \
         --key-id alias/nautilus-KMS-Key \
         --plaintext fileb:///root/SensitiveData.txt \
         --output text \
         --query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
    ```

2. Verify that the encrypted file `EncryptedData.bin` is created in the `/root/` directory.

---

### 3. Decrypt the File
1. Use the following command to decrypt the `EncryptedData.bin` file:
    ```bash
    aws kms decrypt \
         --ciphertext-blob fileb:///root/EncryptedData.bin \
         --output text \
         --query Plaintext | base64 --decode > /root/DecryptedData.txt
    ```

2. Verify that the decrypted file `DecryptedData.txt` matches the original `SensitiveData.txt`.

---

## Validation
1. Ensure the KMS key is correctly configured with the necessary permissions.
2. Run the validation script to test the decryption process:
    ```bash
    ./validation_script.sh
    ```

3. If the script successfully decrypts `EncryptedData.bin` and matches it with `SensitiveData.txt`, the setup is correct.

---

## Notes
- Ensure IAM policies allow access to the KMS key.
- Use the `--region` flag if working in a specific AWS region.
