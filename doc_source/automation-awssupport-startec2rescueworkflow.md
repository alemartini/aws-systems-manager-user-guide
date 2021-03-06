# AWSSupport-StartEC2RescueWorkflow

## Description

The AWSSupport-StartEC2RescueWorkflow automation document runs the provided base64 encoded script (Bash or Powershell) on a helper instance created to rescue your instance. The root volume of your instance is attached and mounted to the helper instance, also known as the EC2Rescue instance.

If your instance is Windows, provide a Powershell script. Otherwise, use Bash.
The workflow sets some environment variables which you can use in your script.
The environment variables contain information about the input you provided, as well as information about the offline root volume. The offline volume is already mounted and ready to use. For example, the Bash script is executed after a chroot to the offline partition is performed.

### Additional Information

To base64 encode a script, you can use:

* **Powershell**

```powershell
[System.Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes([System.IO.File]::ReadAllText('PATH_TO_FILE')))
```

* **Bash**

```bash
base64 PATH_TO_FILE
```

Here is a list of environment variables you can use in your offline scripts, depending on the target OS

#### Windows

| Variable        | Description           | Example value  |
| --------------- |-----------------------| ---------------|
| $env:EC2RESCUE_ACCOUNT_ID | {{ global:ACCOUNT_ID }} | 123456789012 |
| $env:EC2RESCUE_DATE | {{ global:DATE }} | 2018-09-07 |
| $env:EC2RESCUE_DATE_TIME |{{ global:DATE_TIME }} | 2018-09-07_18.09.59 |
| $env:EC2RESCUE_EC2RW_DIR | EC2Rescue for Windows installation path | C:\Program Files\Amazon\EC2Rescue |
| $env:EC2RESCUE_EXECUTION_ID | {{ automation:EXECUTION_ID }}" | 7ef8008e-219b-4aca-8bb5-65e2e898e20b |
| $env:EC2RESCUE_OFFLINE_CURRENT_CONTROL_SET | Offline Windows Current Control Set path | HKLM:\AWSTempSystem\ControlSet001 |
| $env:EC2RESCUE_OFFLINE_DRIVE | Offline Windows drive letter | D:\ |
| $env:EC2RESCUE_OFFLINE_EBS_DEVICE | Offline root volume EBS device | xvdf |
| $env:EC2RESCUE_OFFLINE_KERNEL_VER | Offline Windows Kernel version | 6.1.7601.24214 |
| $env:EC2RESCUE_OFFLINE_OS_ARCHITECTURE | Offline Windows architecture | AMD64 |
| $env:EC2RESCUE_OFFLINE_OS_CAPTION | Offline Windows caption | Windows Server 2008 R2 Datacenter |
| $env:EC2RESCUE_OFFLINE_OS_TYPE | Offline Windows OS type | Server |
| $env:EC2RESCUE_OFFLINE_PROGRAM_FILES_DIR | Offline Windows Program files directory path | D:\Program Files |
| $env:EC2RESCUE_OFFLINE_PROGRAM_FILES_X86_DIR | Offline Windows Program files x86 directory path | D:\Program Files (x86)  |
| $env:EC2RESCUE_OFFLINE_REGISTRY_DIR | Offline Windows registry directory path | D:\Windows\System32\config |
| $env:EC2RESCUE_OFFLINE_SYSTEM_ROOT | Offline Windows system root directory path | D:\Windows |
| $env:EC2RESCUE_REGION | {{ global:REGION }} | us-west-1 |
| $env:EC2RESCUE_S3_BUCKET | {{ S3BucketName }} | mybucket |
| $env:EC2RESCUE_S3_PREFIX | {{ S3Prefix }} | myprefix/ |
| $env:EC2RESCUE_SOURCE_INSTANCE | {{ InstanceId }} | i-abcdefgh123456789 |
| $script:EC2RESCUE_OFFLINE_WINDOWS_INSTALL | Offline Windows Installation metadata | Customer Powershell Object |

#### Linux

| Variable        | Description           | Example value  |
| --------------- |-----------------------| ---------------|
| EC2RESCUE_ACCOUNT_ID | {{ global:ACCOUNT_ID }} | 123456789012 |
| EC2RESCUE_DATE | {{ global:DATE }} | 2018-09-07 |
| EC2RESCUE_DATE_TIME |{{ global:DATE_TIME }} | 2018-09-07_18.09.59 |
| EC2RESCUE_EC2RL_DIR | EC2Rescue for Linux installation path | /usr/local/ec2rl-1.1.3
| EC2RESCUE_EXECUTION_ID | {{ automation:EXECUTION_ID }} | 7ef8008e-219b-4aca-8bb5-65e2e898e20b |
| EC2RESCUE_OFFLINE_DEVICE | Offline device name | /dev/xvdf1 |
| EC2RESCUE_OFFLINE_EBS_DEVICE | Offline root volume EBS device | /dev/sdf |
| EC2RESCUE_OFFLINE_SYSTEM_ROOT | Offline root volume mount point | /mnt/mount |
| EC2RESCUE_PYTHON | Python version | python2.7 |
| EC2RESCUE_REGION | {{ global:REGION }} | us-west-1 |
| EC2RESCUE_S3_BUCKET | {{ S3BucketName }} | mybucket |
| EC2RESCUE_S3_PREFIX | {{ S3Prefix }} | myprefix/ |
| EC2RESCUE_SOURCE_INSTANCE | {{ InstanceId }} | i-abcdefgh123456789 |

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

* **Type**: String
* **Allowed pattern**: ^i-[a-z0-9]{8,17}$
* **Description**: (Required) ID of your EC2 instance. IMPORTANT: AWS Systems Manager Automation stops this instance. Data stored in instance store volumes will be lost. The public IP address will change if you are not using an Elastic IP.

### OfflineScript

* **Type**: String
* **Allowed pattern**: ^[a-zA-Z0-9+/]{3,}[=]{0,2}$
* **Description**: (Required) Base64 encoded script to execute against the helper instance. Use Bash if your source instance is Linux, and PowerShell if it is Windows.

### EC2RescueInstanceType

* **Type**: String
* **Allowed values**: t2.small, t2.medium, t2.large
* **Default**: t2.small
* **Description**: (Optional) The EC2 instance type for the EC2Rescue instance.

### SubnetId

* **Type**: String
* **Allowed values**: ^$|^subnet-[a-z0-9]{8,17}|SELECTED_INSTANCE_SUBNET$
* **Default**: SELECTED_INSTANCE_SUBNET
* **Description**: (Optional) Offline only - The subnet ID for the EC2Rescue instance used to perform the offline troubleshooting. If no subnet ID is specified, AWS Systems Manager Automation will create a new VPC. IMPORTANT: The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints.

### S3BucketName

* **Type**: String
* **Allowed pattern**: ^$|^[_a-zA-Z0-9][-._a-zA-Z0-9]{2,62}$
* **Description**: (Optional) Offline only - S3 bucket name in your account where you want to upload the troubleshooting logs. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs.

### S3Prefix

* **Type**: String
* **Allowed pattern**: ^[a-zA-Z0-9][-./a-zA-Z0-9]{0,255}$
* **Default**: AWSSupport-EC2Rescue
* **Description**: (Optional) A prefix for the S3 logs.

### AMIPrefix

* **Type**: String
* **Allowed pattern**: [-._a-zA-Z0-9]{3,20}$
* **Default**: AWSSupport-EC2Rescue
* **Description**: (Optional) A prefix for the backup AMI name.

### CreatePreEC2RescueBackup

* **Type**: String
* **Allowed values**: True, False
* **Default**: "False
* **Description**: (Optional) Set it to True to create an AMI of InstanceId before executing the script. The AMI will persist after the automation completes. It is your responsibility to secure access to the AMI, or to delete it.

### CreatePostEC2RescueBackup

* **Type**: String
* **Allowed values**: True, False
* **Default**: False
* **Description**: (Optional) Set it to True to create an AMI of InstanceId after executing the script, before starting it. The AMI will persist after the automation completes. It is your responsibility to secure access to the AMI, or to delete it.

### UniqueId

* **Type**: String
* **Default**: {{ automation:EXECUTION_ID }}
* **Description**: (Optional) A unique identifier for the workflow.

### AutomationAssumeRole

* **Type**: String
* **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to execute this document.

## Examples

### Print environment variables on a Windows helper instance

```json
aws ssm start-automation-execution --document-name "AWSSupport-StartEC2RescueWorkflow" --parameters "InstanceId=WINDOWSINSTANCEID,OfflineScript=R2V0LUNoaWxkSXRlbSBlbnY6KiB8IFNvcnQtT2JqZWN0IE5hbWU="
```

### Print environment variables on a Linux helper instance

```json
aws ssm start-automation-execution --document-name "AWSSupport-StartEC2RescueWorkflow" --parameters "InstanceId=LINUXINSTANCEID,OfflineScript=IyEvYmluL2Jhc2gKcHJpbnRlbnYgfCBzb3J0"
```

### Print environment variables on a Windows helper instance and create an AMI before and after the script execution

```json
aws ssm start-automation-execution --document-name "AWSSupport-StartEC2RescueWorkflow" --parameters "InstanceId=WINDOWSINSTANCEID,OfflineScript=R2V0LUNoaWxkSXRlbSBlbnY6KiB8IFNvcnQtT2JqZWN0IE5hbWU=,CreatePreEC2RescueBackup=True,CreatePostEC2RescueBackup=True"
```

### Print environment variables on a Linux helper instance and create an AMI before and after the script execution

```json
aws ssm start-automation-execution --document-name "AWSSupport-StartEC2RescueWorkflow" --parameters "InstanceId=LINUXINSTANCEID,OfflineScript=IyEvYmluL2Jhc2gKcHJpbnRlbnYgfCBzb3J0,CreatePreEC2RescueBackup=True,CreatePostEC2RescueBackup=True"
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended the user who executes the automation have the **AmazonSSMAutomationRole** IAM managed policy attached.
In addition to that policy, the user must have:

```json
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Action": [
            "lambda:InvokeFunction",
            "lambda:DeleteFunction",
            "lambda:GetFunction"
         ],
         "Resource": "arn:aws:lambda:*:An-AWS-Account-ID:function:AWSSupport-EC2Rescue-*",
         "Effect": "Allow"
      },
      {
         "Action": [
            "s3:GetObject",
            "s3:GetObjectVersion"
         ],
         "Resource": [
            "arn:aws:s3:::awssupport-ssm.*/*.template",
            "arn:aws:s3:::awssupport-ssm.*/*.zip"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "iam:CreateRole",
            "iam:CreateInstanceProfile",
            "iam:GetRole",
            "iam:GetInstanceProfile",
            "iam:PutRolePolicy",
            "iam:DetachRolePolicy",
            "iam:AttachRolePolicy",
            "iam:PassRole",
            "iam:AddRoleToInstanceProfile",
            "iam:RemoveRoleFromInstanceProfile",
            "iam:DeleteRole",
            "iam:DeleteRolePolicy",
            "iam:DeleteInstanceProfile"
         ],
         "Resource": [
            "arn:aws:iam::An-AWS-Account-ID:role/AWSSupport-EC2Rescue-*",
            "arn:aws:iam::An-AWS-Account-ID:instance-profile/AWSSupport-EC2Rescue-*"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "lambda:CreateFunction",
            "ec2:CreateVpc",
            "ec2:ModifyVpcAttribute",
            "ec2:DeleteVpc",
            "ec2:CreateInternetGateway",
            "ec2:AttachInternetGateway",
            "ec2:DetachInternetGateway",
            "ec2:DeleteInternetGateway",
            "ec2:CreateSubnet",
            "ec2:DeleteSubnet",
            "ec2:CreateRoute",
            "ec2:DeleteRoute",
            "ec2:CreateRouteTable",
            "ec2:AssociateRouteTable",
            "ec2:DisassociateRouteTable",
            "ec2:DeleteRouteTable",
            "ec2:CreateVpcEndpoint",
            "ec2:DeleteVpcEndpoints",
            "ec2:ModifyVpcEndpoint",
            "ec2:Describe*"
         ],
         "Resource": "*",
         "Effect": "Allow"
      }
   ]
}
```

## Document Version

Document Steps:

1. **aws:executeAwsApi** - Describe the provided instance
1. **aws:executeAwsApi** - Describe the provided instance's root volume
1. **aws:assertAwsResourceProperty** - Check the root volume device type is EBS
1. **aws:assertAwsResourceProperty** - Check the root volume is not encrypted
1. **aws:assertAwsResourceProperty** - Check the provide subnet ID
    1. (Use current instance subnet) - If *SubnetId = SelectedInstanceSubnet*
        1. **aws:createStack** - Deploy the EC2Rescue CloudFormation stack
    1. (Create new VPC) - If *SubnetId = CreateNewVPC*
        1. **aws:createStack** - Deploy the EC2Rescue CloudFormation stack
    1. (Use custom subnet) - In all other cases:
        1. **aws:assertAwsResourceProperty** - Check the provided subnet is in the same Availability Zone as the provided instance
        1. **aws:createStack** - Deploy the EC2Rescue CloudFormation stack
1. **aws:invokeLambdaFunction** - Perfom additional input validation
1. **aws:executeAwsApi** - Update the EC2Rescue CloudFormation stack to creat the EC2Rescue helper instance
1. **aws:waitForAwsResourceProperty** - Wait for the EC2Rescue CloudFormation stack update to complete
1. **aws:executeAwsApi** - Describe the EC2Rescue CloudFormation stack output to obtain the EC2Rescue helper instance ID
1. **aws:waitForAwsResourceProperty** - Wait for the EC2Rescue helper instance to become a managed instance
1. **aws:changeInstanceState** - Stop the provided instance
1. **aws:changeInstanceState** - Force stop the provided instance
1. **aws:assertAwsResourceProperty** - Check the CreatePreEC2RescueBackup input value
    1. (Create pre-EC2Rescue backup) - If *CreatePreEC2RescueBackup = True*
        1. **aws:executeAwsApi** - Create an AMI backup of the provided instance
        1. **aws:createTags** - Tag the AMI backup
1. **aws:runCommand** - Install EC2Rescue on the EC2Rescue helper instance
1. **aws:executeAwsApi** - Detach the root volume from the provided instance
1. **aws:assertAwsResourceProperty** - Check the provided instance platform
    1. (Instance is Windows):
        1. **aws:executeAwsApi** - Attach the root volume to the EC2Rescue helper instance as *xvdf*
        1. **aws:sleep** - Sleep 10 seconds
        1. **aws:runCommand** - Run the provided offline script in Powershell
    1. (Instance is Linux):
        1. **aws:executeAwsApi** - Attach the root volume to the EC2Rescue helper instance as */dev/sdf*
        1. **aws:sleep** - Sleep 10 seconds
        1. **aws:runCommand** - Run the provided offline script in Bash
1. **aws:changeInstanceState** - Stop the EC2Rescue helper instance
1. **aws:changeInstanceState** - Force stop the EC2Rescue helper instance
1. **aws:executeAwsApi** - Detach the root volume from the EC2Rescue helper instance
1. **aws:executeAwsApi** - Attach the root volume back to the provided instance
1. **aws:assertAwsResourceProperty** - Check the CreatePostEC2RescueBackup input value
    1. (Create post-EC2Rescue backup) - If *CreatePostEC2RescueBackup = True*
        1. **aws:executeAwsApi** - Create an AMI backup of the provided instance
        1. **aws:createTags** - Tag the AMI backup
1. **aws:executeAwsApi** - Restore the initial delete on termination state for the root volume of the provided instance
1. **aws:changeInstanceState** - Restore the initial state of the provided instance (running/stopped)
1. **aws:deleteStack** - Delete the EC2Rescue CloudFormation stack