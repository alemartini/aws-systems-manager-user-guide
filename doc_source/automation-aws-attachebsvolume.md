# AWS-AttachEBSVolume

## Description

Attach EBS Volume

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

### Device

- **Type**: String
- **Description**: (Required) The device name (for example, /dev/sdh or xvdh )

### InstanceId

- **Type**: String
- **Description**: (Required) The ID of the instance

### VolumeId

- **Type**: String
- **Description**: (Required) The ID of the EBS volume. The volume and instance must be within the same Availability Zone

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-AttachEBSVolume --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:createStack**
1. **aws:deleteStack**

## Outputs

None
