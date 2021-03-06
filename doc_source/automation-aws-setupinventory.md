# AWS-SetupInventory

## Description

Creates an association for specified instances

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows,Linux

## Parameters

### Applications

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional) Collect data for installed applications.

### AssociatedDocName

- **Type**: String
- **Default**: AWS-GatherSoftwareInventory
- **Description**: (Optional) The name of the Document with which the process associates the EC-2 instances

### AssociationName

- **Type**: String
- **Description**: (Optional) The string applied to the emergent Association value's AssociationName property.

### AssocWaitTime

- **Type**: String
- **Default**: PT5M
- **Description**: (Optional) 8601 Duration of the pause following the beginning of the association process.

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf.

### AwsComponents

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional) Collect data for AWS Components like amazon-ssm-agent.

### CustomInventory

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional) Collect data for custom inventory.

### Files

- **Type**: String
- **Description**: (Optional) (Requires SSMAgent version 2.2.64.0 and above) Linux example: [{"Path":"/usr/bin", "Pattern":["aws*", "*ssm*"],"Recursive":false},{"Path":"/var/log", "Pattern":["amazon*.*"], "Recursive":true, "DirScanLimit":1000}] Windows example: [{"Path":"%PROGRAMFILES%", "Pattern":["*.exe"],"Recursive":true}]

### InstanceDetailedInformation

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional) Collect additional information about the instance, including the CPU model, speed, and the number of cores, to name a few.

### InstanceIds

- **Type**: String
- **Default**: *
- **Description**: (Required) EC2 Instance(s) to associate

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf. If not specified a transient role will be created to execute the Lambda function.

### NetworkConfig

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional) Collect data for Network configurations.

### OutputS3BucketName

- **Type**: String
- **Description**: (Optional) Destination BucketName of the S3Location into which logs will be written

### OutputS3KeyPrefix

- **Type**: String
- **Description**: (Optional) Destination KeyPrefix of the S3 Location into which logs will be written

### OutputS3Region

- **Type**: String
- **Description**: (Optional) Destination Region of the S3 Location into which logs will be written

### Schedule

- **Type**: String
- **Default**: cron(0 */30 * * * ? *)
- **Description**: (Optional) serialized cron expression applied to resultant Association

### Services

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional, Windows OS only, requires SSMAgent version 2.2.64.0 and above) Collect data for service configurations.

### WindowsRegistry

- **Type**: String
- **Description**: (Optional) (Windows OS only, requires SSMAgent version 2.2.64.0 and above) Example: [ {"Path":"HKEY_CURRENT_CONFIG\System","Recursive":true},{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\MachineImage", "ValueNames":["AMIName"]}]

### WindowsRoles

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional, Windows OS only, requires SSMAgent version 2.2.64.0 and above) Collect data for Microsoft Windows role configurations.

### WindowsUpdates

- **Type**: String
- **Default**: Enabled
- **Description**: (Optional, Windows OS only) Collect data for all Windows Updates.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-SetupInventory --parameters 'TODO'
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
1. **aws:sleep**
1. **aws:invokeLambdaFunction**
1. **aws:deleteStack**

## Outputs

None
