# Creating and Testing a DynamoDB Table

## Scenario: Storing and Retrieving User Data

### Step 1: Create a DynamoDB Table
Use the AWS CLI or SDK to create a DynamoDB table named `Users` with a primary key `UserId` (Partition Key).

#### AWS CLI Command:
```bash
aws dynamodb create-table \
    --table-name Users \
    --attribute-definitions AttributeName=UserId,AttributeType=S \
    --key-schema AttributeName=UserId,KeyType=HASH \
    --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```

### Step 2: Insert Test Data
Add a sample user record to the `Users` table.

#### AWS CLI Command:
```bash
aws dynamodb put-item \
    --table-name Users \
    --item '{"UserId": {"S": "123"}, "Name": {"S": "John Doe"}, "Email": {"S": "john.doe@example.com"}}'
```

### Step 3: Retrieve the Data
Fetch the inserted record to verify it was stored correctly.

#### AWS CLI Command:
```bash
aws dynamodb get-item \
    --table-name Users \
    --key '{"UserId": {"S": "123"}}'
```

### Expected Output:
```json
{
    "Item": {
        "UserId": {"S": "123"},
        "Name": {"S": "John Doe"},
        "Email": {"S": "john.doe@example.com"}
    }
}
```

### Step 4: Clean Up
Delete the table after testing to avoid unnecessary costs.

#### AWS CLI Command:
```bash
aws dynamodb delete-table --table-name Users
```

### Notes:
- Ensure AWS CLI is configured with appropriate credentials and region.
- Replace `Users` and attributes with your specific use case if needed.
