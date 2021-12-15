# Install AWS EMP Compatibility Package Builder<a name="emp-install-compatibility-package-builder"></a>

The AWS EMP Compatibility Package Builder is the primary tool used to create EMP compatibility packages to deploy functioning legacy Windows applications on modern Windows Server operating systems on which they would otherwise be incompatible\. Package Builder is provided in an MSI installer file that installs the tool on a clean packaging server\. A *clean packaging server* is a server instance of the Windows operating system version that is supported by the application to be packaged\. 

Perform the following steps to install the AWS EMP Compatibility Package Builder\.

1. If you have not done so already, download AWS EMP Compatibility Package Builder from the [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server product page](http://aws.amazon.com/emp-windows-server)\.

   New releases of the AWS End\-of\-Support Migration Program \(EMP\) for Windows Server Compatibility Package Builder are provided in MSI format\. To upgrade to a new version from a previous version, uninstall the previous version by using the **Add or Remove Programs** feature from legacy Windows operating systems, or from **Programs and Features** in the **Control Panel** for later operating systems\. Then, reinstall the package with the latest MSI\.

1. After you have downloaded the EMP tools, double\-click the Compatibility Package Builder file to run it\.

1. On the **Welcome to the Compatibility Package Builder Setup Wizard** pop\-up, choose **Next**\.

1. In the **End\-User License Agreement**, select the terms agreement, and choose **Next**\.

1. Under **EMP Telemetry**, select the check box to enable telemetry \(optional\), and choose **Next**\.

1. Accept the default **Destination Folder**, or modify it, and choose **Next**\.

1. Choose **Install**\.

1. When the application installation completes, choose **Finish**\.