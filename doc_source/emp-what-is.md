# What Is AWS End\-of\-Support Migration Program \(EMP\) for Windows Server?<a name="emp-what-is"></a>

The AWS End\-of\-Support Migration Program \(EMP\) for Windows Server provides the technology and guidance to migrate your applications running on Windows Server 2003, Windows Server 2008, and Windows Server 2008 R2 to the latest, supported versions of Windows Server running on Amazon Web Services \(AWS\)\. Using EMP technology, you can decouple critical applications from their underlying operating system so that they can be migrated to a supported version of Windows Server on AWS without code changes\.

**Topics**
+ [Features](#emp-features)
+ [Concepts](#emp-concepts)
+ [Accessing](#emp-accessing)
+ [Pricing](#emp-pricing)

## Features of AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-features"></a>

The AWS End\-of\-Support Migration Program \(EMP\) for Windows Server toolset offers the following features to help you migrate your applications running on legacy Windows Server operating systems to supported versions\. 

**Existing installation capture**  
During packaging, EMP tools capture the existing installation of a legacy application to an EMP package\. The EMP tools typically perform this by detecting the components of an existing installation and extracting them to an EMP package\. When installation media and knowledge are available, the installation can be monitored and captured to an EMP package\.

**Fresh installation capture**  
During packaging, a snapshot is taken of the current state of the operating system, and a capture process records all of the changes made during the application installation\.

**Runtime analysis**  
File and registry access, along with changes that occur when an application is launched from this section of the packing wizard, are recorded and made available for addition to the package being created\. This feature allows EMP to include first\-launch changes by the application to the system\.

**Legacy operating system support**  
An EMP package is created on the operating system that is currently supported by the application being packaged\. This compatibility packaging allows you to migrate an application from a compatible operating system to an incompatible operating system, while preserving application functionality\.

**Windows upgrade support**  
You can deploy a single package across multiple supported Windows versions\. In addition, each package supports the upgrade of the underlying version of Windows version\.

**Pre\-deployment and post\-deployment scripts**  
You can include custom scripts in the package to deploy scripted installations, configurations, or to run other required tasks for the package and application deployment process\.

**Fast editing with no package recompilation**  
Test and remediation cycles for EMP packages take less time as a result of fast editing, which requires no package recompilation\. Packages can be updated to include specific application updates or the latest EMP components without having to recompile the package\.

## AWS End\-of\-Support Migration Program \(EMP\) for Windows Server concepts<a name="emp-concepts"></a>

The following concept is central to your understanding and use of AWS End\-of\-Support Migration Program \(EMP\) for Windows Server\.

**Packaging**  
The process of capturing a legacy application using the EMP tools on the source operating system\.

**Source server**  
The server instance on which the application to be packaged is currently installed and running\. 

**Clean packaging server**  
The server instance of the Windows operating system version that is supported by the application to be packaged\. This server instance is used in the fresh install capture of an application\.

**Compatibility package**  
When the EMP compatibility packaging process is complete, the output of the package builder is called an EMP compatibility package\. 

## Accessing AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-accessing"></a>

AWS End\-of\-Support Migration Program \(EMP\) for Windows Server offers standalone tools that you download and install on your source or packaging instance\. You can download the EMP tools from the [End\-of\-Support Migration Program for Windows Server product page](http://aws.amazon.com/emp-windows-server/)\.

New releases of the AWS End\-of\-Support Migration Program \(EMP\) for Windows Server Compatibility Package Builder are provided in MSI format\. To upgrade to a new version from a previous version, uninstall the previous version by using the **Add or Remove Programs** feature from legacy Windows operating systems, or from **Programs and Features** in the **Control Panel** for later operating systems\. Then, reinstall the package with the latest MSI\.

## Pricing for EMP<a name="emp-pricing"></a>

AWS End\-of\-Support Migration Program \(EMP\) for Windows Server is available for use at no cost to support migration of Windows workloads to the AWS Cloud\.