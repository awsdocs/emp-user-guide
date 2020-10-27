# Applications using ODBC drivers<a name="emp-applications-odbc"></a>

Migrate applications that require Open Database Connectivity \(ODBC\) drivers\.

**Microsoft ODBC drivers**  
Windows Server 2016 and Windows Server 2019 include several Microsoft ODBC drivers installed as part of the operating system\. We recommend that you use these versions in the package instead of earlier versions\.

To verify whether an ODBC driver is present on the target operating system, open the **Drivers** tab of the ODBC Data Source Administrator application\. Note that there are separate 32\-bit and 64\-bit versions of the ODBC data sources and drivers\.

![\[ODBC drivers installed on system\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-odbc.png)

You can also query for the installed ODBC drivers using PowerShell by running the `Get-ODBCDriver` cmdlt\.

**Third\-party ODBC drivers**  
Applications that require ODBC drivers that are not included by default in Windows operating systems must be included within the EMP package so that the application has everything it requires to run\. Applications that install third\-party ODBC drivers as part of the application install process will include the ODBC drivers in the EMP package as part of the install capture process\. Applications that require ODBC drivers to be manually installed, either before or after the application is installed, must be installed during the install capture process so that they are included in the EMP package\.
