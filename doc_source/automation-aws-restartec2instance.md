# AWS-RestartEC2Instance

## Description

Restart EC2 instances(s)

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

- **Type**: StringList
- **Description**: (Required) EC2 Instance(s) to restart

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-RestartEC2Instance --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:changeInstanceState**
1. **aws:changeInstanceState**

## Outputs

None
