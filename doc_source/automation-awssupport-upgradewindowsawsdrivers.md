# AWSSupport-UpgradeWindowsAWSDrivers

## Description

The AWSSupport-UpgradeWindowsAWSDrivers upgrades or repairs storage and network AWS drivers on the specified EC2 instance. The document attempts to install the latest versions of AWS drivers online by calling the SSM agent. If the SSM agent is not contactable, the document can perform an offline installation of the AWS drivers if explicitly requested. Note: Both the online and offline upgrade will create an AMI before attempting any operations, which will persist after the automation completes. It is your responsibility to secure access to the AMI, or to delete it. The online method restarts the instance as part of the upgrade process, while the offline method requires the provided EC2 instance be stopped and then started.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

- **Type**: String
- **Allowed values**: ^i-[a-z0-9]{8,17}$
- **Description**: (Required) ID of your EC2 instance.

### AllowOffline

- **Type**: String
- **Allowed values**: True, False
- **Default**: False
- **Description**: (Optional) Set it to true if you allow an offline drivers upgrade in case the online installation cannot be performed. Note: The offline method requires the provided EC2 instance be stopped and then started. Data stored in instance store volumes will be lost. The public IP address will change if you are not using an Elastic IP.

### SubnetId

- **Type**: String
- **Allowed values**: ^$|^subnet-[a-z0-9]{8,17}$
- **Description**: (Optional) Offline only - The subnet ID for the EC2Rescue instance used to perform the offline drivers upgrade. If no subnet ID is specified, AWS Systems Manager Automation will create a new VPC. IMPORTANT: The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints.

### ForceUpgrade

- **Type**: String
- **Allowed values**: True, False
- **Default**: False
- **Description**: (Optional) Offline only - Set it to true if you allow the offline drivers upgrade to proceed even though your instance already has the latest drivers installed.

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to execute this document.

## Examples

### Start the automation

```json
aws ssm start-automation-execution --document-name AWSSupport-UpgradeWindowsAWSDrivers --parameters "InstanceId=INSTANCEID"
```

### Start the automation and allow an offline upgrade

```json
aws ssm start-automation-execution --document-name AWSSupport-UpgradeWindowsAWSDrivers --parameters "InstanceId=INSTANCEID,AllowOffline=True"
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached.
You must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to execute the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output.
If you are performing an offline upgrade, see the permissions required by *AWSSupport-StartEC2RescueWorkflow*.

## Document Version

Document Steps:

1. **aws:assertAwsResourceProperty** - Verifies the input instance is Windows.
1. **aws:assertAwsResourceProperty** - Verifies the input instance is a managed instance. If so, the online upgrade starts, otherwise the offline upgrade is evaluated.
    1. (Online upgrade) If the input instance is a managed instance:
        1. **aws:createImage** - Creates an AMI backup.
        1. **aws:createTags** - Tags the AMI backup.
        1. **aws:runCommand** - Installs ENA network driver via AWS-ConfigureAWSPackage.
        1. **aws:runCommand** - Installs NVMe driver via AWS-ConfigureAWSPackage.
        1. **aws:runCommand** - Installs AWS PV driver via AWS-ConfigureAWSPackage.
    1. (Offline upgrade) If the input instance is not a managed instance:
        1. **aws:assertAwsResourceProperty** - Verifies the AllowOffline flag is set to True. If so, the offline upgrade starts, otherwise the workflow ends.
        1. **aws:changeInstanceState** - Stop the source instance.
        1. **aws:changeInstanceState** - Force-stop the source instance.
        1. **aws:createImage** - Create an AMI backup of the source instance.
        1. **aws:createTags** - Tag the AMI backup of the source instance.
        1. **aws:executeAwsApi** - Enable ENA for the instance
        1. **aws:assertAwsResourceProperty** - Assert the ForceUpgrade flag.
            1. (Force offline upgrade) If *ForceUpgrade = True*
                1. **aws:executeAutomation** - Invoke AWSSupport-StartEC2RescueWorkflow with the drivers force upgrade script. This installs the drivers regardless of the current version that is installed
            1. (Offline upgrade) If *ForceUpgrade = False*
                1. **aws:executeAutomation** - Invoke AWSSupport-StartEC2RescueWorkflow with the drivers upgrade script.
