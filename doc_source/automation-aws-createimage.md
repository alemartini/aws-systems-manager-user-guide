# AWS-CreateImage

## Description

Creates a new Amazon Machine Image (AMI) from an Amazon EC2 instance

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
- **Description**: (Required) The ID of the Amazon EC2 instance.

### NoReboot

- **Type**: Boolean
- **Description**: (Optional) Do not reboot the instance before creating the image.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-CreateImage --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:createImage**

## Outputs

createImage.ImageId
