# Deploy an EMP package<a name="emp-deploy"></a>

This topic contains information and steps to set up for and deploy an EMP package\.

**Topics**
+ [Requirements](#emp-deploy-requirements)
+ [Run deployment tool](#emp-deploy-deployment-tool)

## Requirements for deploying an EMP package<a name="emp-deploy-requirements"></a>

When you deploy an EMP package on Windows Server 2012 or later, AWS credentials and connectivity to the AWS application modernization metrics service are required\. The deployment fails if the package cannot validate the supplied AWS credentials or cannot send the mandatory telemetry to AWS\. 

There are two ways you can provide AWS credentials to the deployment package if they are not set up on the server\.
+ If your deployment server is not an Amazon EC2 instance, you can configure the AWS profile on the server and update the profile name in the `metadata.json` file in the root of the packaged folder\.
+ If your deployment server is an Amazon EC2 instance, you can assign an `execute-api:Invoke` IAM role to the server\.

**Configure the AWS profile on the server \(server is not an EC2 instance\)**  
You can configure the AWS profile on the server using the AWS CLI or AWS Tools for Windows PowerShell\. You must set up an IAM user in your AWS environment\. The IAM user must be configured to allow `execute-api:Invoke`\. To configure this, assign the following IAM policy to the IAM user of the AWS profile on the server\.

```
{ "Version": "2012-10-17", "Statement": [{ "Effect": "Allow", "Action": "execute-api:Invoke", "Resource": "*" }]} 
```

**AWS CLI**

When you use the AWS CLI, profile information is stored in the `C:\Users\<username>\.aws` directory\. Use the `aws configure` command to configure the profile for the IAM user you set up to allow for `execute-api:Invoke` permissions\. The AWS CLI can be downloaded from the [Installing, updating, and uninstalling the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) page\. For more information about how to specify a profile using the AWS CLI, see [Named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)\.

AWS CLI example

```
C:\>aws configure
AWS Access Key ID [None]: <EXAMPLE-ACCESSKEY>
AWS Secret Access Key [None]: <EXAMPLE-SECRETKEY>
Default region name [None]: 
Default output format [None]:

C:\>
```

**AWS Tools for Windows PowerShell**

When you use AWS Tools for Windows PowerShell, profile information is stored in `C:\Users\<username>\AppData\Local\AWSToolkit\RegisteredAccounts.json`\. Use the`Set-AWSCredential` command to configure the profile\. For more information about how to specify credentials using AWS Tools for PowerShell, see [Using AWS credentials](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) in the *AWS Tools for PowerShell User Guide*\.

AWS Tools for Windows PowerShell example

```
PS C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowerShell> Set-AWSCredential -AccessKey <EXAMPLE-ACCESSKEY> -SecretKey <EXAMPLE-SECRETKEY> -StoreAs default 
PS C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowerShell>
```

**Note**  
If you do not specify a name when you create a profile, it will default to `default` by both the AWS CLI and AWS Tools for PowerShell\. You are not required to update the `metadata.json` file found in the root of the EMP package\. If you specify a new name for the profile at a later time, update the `AWSProfileName` property in the `metadata.json` file\.

**Assign IAM role to the server \(server is an EC2 instance\)**  
Assign an IAM role to the deployment server and verify that the following IAM policy is applied to it\. For more information about how to assign an IAM role, see [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html)\.

```
{ "Version": "2012-10-17", "Statement": [{ "Effect": "Allow", "Action": "execute-api:Invoke", "Resource": "*" }]} 
```

**Connectivity to the AWS application modernization metrics service**  
The EMP deployment server must have internet connectivity to create a secure `http` outbound connection to the AWS application modernization service\. 

## Run the deployment tool<a name="emp-deploy-deployment-tool"></a>

To run the deployment tool, perform the following steps\.

1. Open a command prompt as an administrator\.

1. Run the following command to deploy the package to all of the users of the server, where `<path-to-package>` is the path of the EMP package, and `<switches>` are the relevant command line switches you want to specify\.

   ```
   <path-to-package>\Compatibility.Package.Deployment.Exe /<switches>
   ```

   For example:

   ```
   C:\EMP\Package0001\Compatibility.Package.Deployment.Exe /acceptEULA /deploydir "C:\Programdata\EMP" /DeployAllRegistry 
   ```

   When you run this command, the following operations are performed\.
   + All files in the `<path_to_package>` folder are copied to the specified `/deploydir`\. 
   + All shortcuts specified in the `shortcuts.xml` are written to the public profile for visibility to all users of the server\. 
   + Shortcuts for a path in the user desktop or **Start** menu are translated to the equivalent of the directory of the public profile\. 
   + Any file type associations specified in the `FileAssociation.xml` are created in HKLM root key of the registry\. 
   + If the `/deploydir` switch is provided, the package is copied to the specified folder\. 
   + The `/DeployAllRegistry` switch makes the EMP package accessible for all users at the machine level\.