# AWS-AttachIAMToInstance

## Description

Attach IAM to Instance

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
- **Description**: (Required) The ID of the instance.

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role assumed by lambda

### RoleName

- **Type**: String
- **Description**: (Required) Role Name to add

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-AttachIAMToInstance --parameters 'TODO'
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
1. **aws:deleteStack**

## Outputs

attachIAMToInstance.LogResult
attachIAMToInstance.Payload
