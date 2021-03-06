# AWS-CreateDynamoDbBackup

## Description

Create DynamoDB table backup

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

### BackupName

- **Type**: String
- **Description**: (Required) Name of the backup to create.

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf. If not specified a transient role will be created to execute the Lambda function.

### TableName

- **Type**: String
- **Description**: (Required) Name of the DynamoDB Table.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-CreateDynamoDbBackup --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:createStack**
1. **aws:invokeLambdaFunction**
1. **aws:invokeLambdaFunction**
1. **aws:deleteStack**

## Outputs

createDynamoDbBackup.Payload
