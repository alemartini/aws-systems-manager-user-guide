# AWS-DeleteCloudFormationStackWithApproval

## Description

Delete CloudFormation Stack with approval

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows,Linux

## Parameters

### Approvers

- **Type**: StringList
- **Description**: (Required) IAM user or user arn of approvers for the automation action

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.

### SNSTopicArn

- **Type**: String
- **Description**: (Required) The SNS topic ARN used to send pending approval notification for delete CloudFormation Stack. The SNS topic name must start with Automation.

### StackNameOrId

- **Type**: String
- **Description**: (Required) Name or Unique ID of the CloudFormation stack to be deleted

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-DeleteCloudFormationStackWithApproval --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:approve**
1. **aws:deleteStack**

## Outputs

None
