# AWS-SetupManagedInstance

## Description

Manage EC2 instance

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

### InstanceId

- **Type**: String
- **Description**: (Required) EC2 InstanceId to manage

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf. If not specified a transient role will be created to execute the Lambda function.

### RoleName

- **Type**: String
- **Default**: SSMRoleForManagedInstance
- **Description**: (Optional) The name of the IAM Role for the EC2 Instance. If this Role does not exist, it will be created. When specifying this value, the caller should ensure the Role contains the AWS Managed Policy "AmazonEC2RoleForSsm".

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-SetupManagedInstance --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:executeAutomation**
1. **aws:runCommand**
1. **aws:executeAutomation**

## Outputs

None
