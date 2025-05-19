# Expanding EC2 Storage Volume

This guide provides a step-by-step process to expand the storage volume of an EC2 instance. You can achieve this via the AWS Management Console or the AWS CLI.

---

## Using AWS Management Console

1. **Navigate to the EC2 Dashboard**:
    - Open the AWS Management Console and go to the EC2 service.

2. **Select the Volume**:
    - Under the "Elastic Block Store" section, click on "Volumes".
    - Locate and select the volume you want to expand.

3. **Modify the Volume**:
    - Click on the "Actions" dropdown and select "Modify Volume".
    - Enter the new desired size for the volume and confirm the changes.

4. **Wait for the Modification to Complete**:
    - The volume will enter a "modifying" state. Wait until the state changes to "available".

---

## Using AWS CLI

1. **Modify the Volume**:
    Run the following command to modify the volume size:
    ```bash
    aws ec2 modify-volume --volume-id <volume-id> --size <new-size>
    ```
    Replace `<volume-id>` with the ID of your volume and `<new-size>` with the desired size in GiB.

2. **Verify the Modification**:
    Use the following command to check the status:
    ```bash
    aws ec2 describe-volumes-modifications --volume-id <volume-id>
    ```

---

## Resize the Filesystem

After modifying the volume, you need to resize the filesystem to utilize the expanded space.

1. **Check the Block Devices**:
    ```bash
    lsblk
    ```
    Identify the device name (e.g., `/dev/xvda`).

2. **Resize the Partition**:
    - If `growpart` is available:
      ```bash
      sudo growpart /dev/xvda 1
      ```
      Replace `/dev/xvda` with the actual device name.

    - If `growpart` is not available, use `parted`:
      ```bash
      sudo parted /dev/xvda resizepart 1 <new-size>
      ```
      Replace `/dev/xvda` with the actual device name and `<new-size>` with the new size (e.g., `12G`).

3. **Resize the Filesystem**:
    - For XFS filesystems:
      ```bash
      sudo xfs_growfs /
      ```
    - For ext4 filesystems:
      ```bash
      sudo resize2fs /dev/xvda1
      ```
      Replace `/dev/xvda1` with the actual partition name.

4. **Verify the Changes**:
    ```bash
    df -h
    ```
    Ensure the updated size is reflected.

---

By following these steps, you can successfully expand the storage volume of your EC2 instance and make the additional space available for use.  