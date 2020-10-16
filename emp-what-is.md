# What Is AWS End\-of\-Support Migration Program \(EMP\) for Windows Server?<a name="emp-what-is"></a>

The AWS End\-of\-Support Migration Program \(EMP\) for Windows Server provides the technology and guidance to migrate your applications running on Windows Server 2003, Windows Server 2008, and Windows Server 2008 R2 to the latest, supported versions of Windows Server running on Amazon Web Services \(AWS\)\. Using EMP technology, you can decouple critical applications from their underlying operating system so that they can be migrated to a supported version of Windows Server on AWS without code changes\.

**Topics**
+ [Features](#emp-features)
+ [Concepts](#emp-concepts)
+ [Supported operating systems and requirements](#emp-what-is-supported-os)
+ [Accessing](#emp-accessing)
+ [Pricing](#emp-pricing)

## Features of AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-features"></a>

 AWS End\-of\-Support Migration Program \(EMP\) for Windows Server offers the following features to help you migrate your applications running on legacy Windows Server operating systems to supported versions\. 

**Install capture**  
During packaging, a snapshot is taken of the current state of the operating system and a capture process starts recording all changes made during the application installation\.

**Runtime analysis**  
File and registry access, along with changes that occur when an application is launched from this section of the packing wizard, are recorded and made available for addition to the package being created\. This feature allows EMP to include first\-launch changes by the application to the system\.

**Legacy OS support**  
An EMP package is created on the operating system that is currently supported by the application being packaged\. This compatibility packaging allows you to migrate an application from an operating system with which it is compatible to one with which it is incompatible, while preserving application functionality\.

**Cross\-platform support**  
You can deploy a single package across multiple supported operating systems\.

**Pre\- and post\- deployment scripts**  
You can include custom scripts in the package to deploy MSI \(Microsoft Installer\) files or to run other tasks that must be performed as part of the package/application deployment process\.

**Fast editing with no package recompilation**  
The time it takes to package, test, and update applications and configurations is reduced with fast editing, which requires no package recompilation\. EMP packages can be updated to include application updates or the latest EMP components\.

## AWS End\-of\-Support Migration Program \(EMP\) for Windows Server concepts<a name="emp-concepts"></a>

The following concept is central to your understanding and use of AWS End\-of\-Support Migration Program \(EMP\) for Windows Server\.

**Packaging**  
The process of capturing a legacy application using the EMP package builder on the source operating system\.

**Clean packaging server**  
A server instance of the Windows operating system version that is supported by the application to be packaged\. 

## Supported operating systems and requirements<a name="emp-what-is-supported-os"></a>

For supported operating systems and requirements to successfully migrate your application with AWS End\-of\-Support Migration Program \(EMP\) for Windows Server, see [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server System requirements](emp-supported-os.md)\.

## Accessing AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-accessing"></a>

AWS End\-of\-Support Migration Program \(EMP\) for Windows Server offers standalone tools that you download and install on your developer workstation\. You can download the EMP tools from the [End\-of\-Support Migration Program for Windows Server product page](http://aws.amazon.com/emp-windows-server/)\.

New releases of the AWS End\-of\-Support Migration Program \(EMP\) for Windows Server Compatibility Packager are provided in MSI format\. To upgrade to a new version from a previous version, uninstall the previous version by using the **Add or Remove Programs** feature from legacy Windows operating systems, or from **Programs and Features** in the **Control Panel** for later operating systems\. Then, reinstall the package with the latest MSI\.

## Pricing for EMP<a name="emp-pricing"></a>

AWS End\-of\-Support Migration Program \(EMP\) for Windows Server is available for use at no cost\.
