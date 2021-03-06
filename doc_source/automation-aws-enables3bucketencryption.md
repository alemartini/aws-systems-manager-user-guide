# AWS-EnableS3BucketEncryption

## Description

Enables Encryption on S3 Bucket

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows,Linux

## Parameters

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.

### BucketName

- **Type**: String
- **Description**: (Required) The name of the S3 Bucket whose content will be encrypted.

### SSEAlgorithm

- **Type**: String
- **Default**: AES256
- **Description**: (Optional) Server-side encryption algorithm to use for the default encryption.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-EnableS3BucketEncryption --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:executeAwsApi**

## Outputs

None
