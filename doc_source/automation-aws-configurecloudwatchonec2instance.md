# AWS-ConfigureCloudWatchOnEC2Instance

## Description

Configure CloudWatch on Instance

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
- **Description**: (Required) InstanceId of the EC2 instance to configure

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role assumed by lambda

### properties

- **Type**: String
- **Description**: (Optional) The configuration for CloudWatch in JSON format.

### status

- **Type**: String
- **Allowed values**: Enabled,Disabled
- **Default**: Enabled
- **Description**: (Optional) Specifies whether to enable or disable CloudWatch. Valid values: "Enabled" | "Disabled"

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-ConfigureCloudWatchOnEC2Instance --parameters 'TODO'
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

None
