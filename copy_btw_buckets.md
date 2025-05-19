# Copying Data Between S3 Buckets

This guide explains how to copy data between S3 buckets in the same region and across different regions, along with a script to verify data integrity.

## Prerequisites
1. AWS CLI installed and configured with appropriate permissions.
2. Source and destination bucket names.

---

## Steps to Copy Data Between Buckets

### 1. Copy Data Within the Same Region
Use the `aws s3 cp` command with the `--recursive` flag to copy all objects.

```bash
aws s3 cp s3://source-bucket/ s3://destination-bucket/ --recursive
```

### 2. Copy Data Across Different Regions
If the buckets are in different regions, the same command works. Ensure the destination bucket exists in the target region.

```bash
aws s3 cp s3://source-bucket/ s3://destination-bucket/ --recursive
```

---

## Verifying Data Integrity

### 1. Generate Checksums for Source Bucket
Run the following script to generate MD5 checksums for all objects in the source bucket:

```bash
aws s3api list-objects --bucket source-bucket --query "Contents[].Key" --output text | while read key; do
    aws s3api head-object --bucket source-bucket --key "$key" --query "ETag" --output text | tr -d '"' >> source_checksums.txt
done
```

### 2. Generate Checksums for Destination Bucket
Repeat the same process for the destination bucket:

```bash
aws s3api list-objects --bucket destination-bucket --query "Contents[].Key" --output text | while read key; do
    aws s3api head-object --bucket destination-bucket --key "$key" --query "ETag" --output text | tr -d '"' >> destination_checksums.txt
done
```

### 3. Compare Checksums
Use the `diff` command to compare the checksum files:

```bash
diff source_checksums.txt destination_checksums.txt
```

If there is no output, the data was copied successfully without loss.

---

## Notes
- Ensure proper IAM permissions for both buckets.
- For large datasets, consider using `aws s3 sync` for better performance:

```bash
aws s3 sync s3://source-bucket/ s3://destination-bucket/
```

- Always test in a non-production environment before executing on critical data.
