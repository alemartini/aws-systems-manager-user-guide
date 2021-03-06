# AWS-UpdateWindowsAmi

## Description

Updates a Microsoft Windows AMI. By default it will install all Windows updates, Amazon software, and Amazon drivers. It will then sysprep and create a new AMI. Supports Windows Server 2008 R2 and greater.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows,Linux

## Parameters

### AutomationAssumeRole

- **Type**: String
- **Default**: arn:aws:iam::{{global:ACCOUNT_ID}}:role/AutomationServiceRole
- **Description**: (Required) The ARN of the role that allows Automation to perform the actions on your behalf.

### Categories

- **Type**: String
- **Description**: (Optional) Specify one or more update categories. You can filter categories using comma-separated values. Options: Application, Connectors, CriticalUpdates, DefinitionUpdates, DeveloperKits, Drivers, FeaturePacks, Guidance, Microsoft, SecurityUpdates, ServicePacks, Tools, UpdateRollups, Updates. Valid formats include a single entry, for example: CriticalUpdates. Or you can specify a comma separated list: CriticalUpdates,SecurityUpdates. NOTE: There cannot be any spaces around the commas.

### ExcludeKbs

- **Type**: String
- **Description**: (Optional) Specify one or more Microsoft Knowledge Base (KB) article IDs to exclude. You can exclude multiple IDs using comma-separated values. Valid formats: KB9876543 or 9876543.

### IamInstanceProfileName

- **Type**: String
- **Default**: ManagedInstanceProfile
- **Description**: (Required) The name of the role that enables Systems Manager to manage the instance.

### IncludeKbs

- **Type**: String
- **Description**: (Optional) Specify one or more Microsoft Knowledge Base (KB) article IDs to include. You can install multiple IDs using comma-separated values. Valid formats: KB9876543 or 9876543.

### InstanceType

- **Type**: String
- **Default**: t2.medium
- **Description**: (Optional) Type of instance to launch as the workspace host. Instance types vary by region. Default is t2.medium.

### PostUpdateScript

- **Type**: String
- **Description**: (Optional) A script provided as a string. It will execute after installing OS updates.

### PreUpdateScript

- **Type**: String
- **Description**: (Optional) A script provided as a string. It will execute prior to installing OS updates.

### PublishedDateAfter

- **Type**: String
- **Description**: (Optional) Specify the date that the updates should be published after.  For example, if 01/01/2017 is specified, any updates that were found during the Windows Update search that have been published on or after 01/01/2017 will be returned.

### PublishedDateBefore

- **Type**: String
- **Description**: (Optional) Specify the date that the updates should be published before.  For example, if 01/01/2017 is specified, any updates that were found during the Windows Update search that have been published on or before 01/01/2017 will be returned.

### PublishedDaysOld

- **Type**: String
- **Description**: (Optional) Specify the amount of days old the updates must be from the published date.  For example, if 10 is specified, any updates that were found during the Windows Update search that have been published 10 or more days ago will be returned.

### SeverityLevels

- **Type**: String
- **Description**: (Optional) Specify one or more MSRC severity levels associated with an update. You can filter severity levels using comma-separated values. By default patches for all security levels are selected. If value supplied, the update list is filtered by those values. Options: Critical, Important, Low, Moderate or Unspecified. Valid formats include a single entry, for example: Critical. Or, you can specify a comma separated list: Critical,Important,Low.

### SourceAmiId

- **Type**: String
- **Description**: (Required) The source Amazon Machine Image ID.

### SubnetId

- **Type**: String
- **Description**: (Optional) Specify the SubnetId if you want to launch into a specific subnet.

### TargetAmiName

- **Type**: String
- **Default**: UpdateWindowsAmi_from_{{SourceAmiId}}_on_{{global:DATE_TIME}}
- **Description**: (Optional) The name of the new AMI that will be created. Default is a system-generated string including the source AMI id, and the creation time and date.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWS-UpdateWindowsAmi --parameters 'TODO'
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

TODO

## Document Steps

1. **aws:runInstances**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:runCommand**
1. **aws:changeInstanceState**
1. **aws:createImage**
1. **aws:changeInstanceState**

## Outputs

CreateImage.ImageId
