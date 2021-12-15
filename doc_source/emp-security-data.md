# Data collected by the AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-security-data"></a>

AWS collects usage information through the EMP telemetry module during the deployment and subsequent use of EMP packages\. Collection of the telemetry collected during deployment is mandatory, whereas the runtime \(usage\) telemetry is optional\. You can opt out of sending runtime telemetry when you install the EMP Compatibility Package Builder\. The telemetry module sends the collected data to an application modernization metrics service running on AWS\. 

The following mandatory telemetry metrics are collected by AWS:
+ AWS account ID
+ Windows operating system version
+ Infrastructure \(AWS or not AWS\)
+ Hashed package ID

The following optional runtime telemetry metrics are collected by AWS:
+ Windows operating system, for example, `Windows Server 2008`\.
+ Windows operating system architecture, for example, `x86`\.
+ Instance type\. The instance type is the machine type on which the application is deployed\. It can be either an Amazon EC2 instance or an on\-premises machine\.
+ EMP compatibility engine version, for example, `1.0.0.4`\.
+ EMP compatibility engine architecture, for example `x86`\.
+ Virtualized executable name, for example, `Notepad++`\.
+ Virtualized Application Architecture, for example, `x86`\.
+ Namespace to identify whether the package run is a development or production run\.