# AWS-DeleteEbsVolumeSnapshots

## Description

Delete EBS Volume snapshots

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

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf. If not specified a transient role will be created to execute the Lambda function.

### RetentionCount

- **Type**: String
- **Default**: 10
- **Description**: (Optional) Number of snapshots to keep for the volume.  Either RetentionCount or RetentionDays should be mentioned, not both.

### RetentionDays

- **Type**: String
- **Description**: (Optional) Number of days to keep snapshots for the volume. Either RetentionCount or RetentionDays should be mentioned, not both

### VolumeId

- **Type**: String
- **Description**: (Required) The volume identifier to delete snapshots for.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-DeleteEbsVolumeSnapshots --parameters 'TODO'
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

deleteVolumeSnapshots.Payload
