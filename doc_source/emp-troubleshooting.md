# Troubleshooting AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-troubleshooting"></a>

When troubleshooting AWS End\-of\-Support Migration Program \(EMP\) for Windows Server issues, we recommend that you apply the following methodology\. There are four major types of issues related to EMP packaging\. 
+ **Environment** — The target system is running in production and may be subject to more stringent restrictions than the test server\. Common examples include more restrictive group policies, antivirus restrictions, enabled controlled folders, and User Access Control restrictions\.
+ **Legacy application** — It is not uncommon to find pre\-existing issues in the application that exist before packaging\. For example, if a function fails in the packaged application you should test the functionality on the natively installed application on the legacy server\.
+ **Packaging** — These are issues related to the capture process and primarily an incomplete test plan\. A common example would be functionality that should have been run during the capture, but was not included and therefore system calls to the registry or files was not captured\.
+ **EMP packaging tools** — These are issues related to the EMP product\. If you suspect product issues, create a ticket so the product team can look into the issue\. 

To find the root cause of an issue, we recommend the following workflow, where you begin testing on the legacy operating system and then move towards testing on the target operating system\.

1. Test the package on a clean machine running the source operating system and where the application has never been installed\. A clean testing machine is a server instance of the Windows operating system version with as little else as possible installed on the machine\. 

1. Test the package on a machine running the target operating system where no other applications or policies are installed\. 

1. Test the package on a machine running the target operating system configured with other applications and policies that will exist in the target environment\.

This testing process allows you to identify the step during which the package is failing\. 

When the package fails during testing, we recommend trying the following tools to remediate\. 
+ **Check if any error messages appear when the application fails** — An application failure typically results in an error message\. The error message may be from Windows and provide details about what has gone wrong, or it may provide a Windows error code, which can help to diagnose the problem\. The message can also be from the application process that encountered the problem\. This indicates that the process was successfully started then encountered a problem\. Application error codes and other messages indicate that the error has been logged and assist with problem diagnoses\.
+ **Check the Windows Event Viewer for application errors** — Application errors are not always logged in Windows Event Logs; however, we recommend that you check them for useful diagnostic information\.
+ **Use the Sysinternals tool, Process Explorer** — Use Process Explorer to check the command line and environment variables used to load the application process\.

  1. Download Process Explorer from [https://docs\.microsoft\.com/en\-us/sysinternals/downloads/process\-explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)\.

  1. Launch your EMP packaged application or reproduce the error you want to investigate\.

  1. Launch Process Explorer and look for the process you want to investigate\.

  1. Open \(double\-click\) the process that you want to investigate to open the **Properties** window\. 

  1. View the command line on the **Image** tab of the **Properties** window, in the **Command line** box\.

  1. In the **Properties** window, choose the **Environment** tab to view the environment variables that are available to the process\.
+ **Use the Sysinternals tool, Process Monitor \(procmon\)** — Check Process Monitor to see if the application writes any log files to the drive to investigate\. Also use Process Monitor to check if there are any registry keys or files the application is not able to find \(the result returned for this scenario is `PATH NOT FOUND`\)\. Or check to see if the application is failing to read or write from registry keys or files even when found \(the returned result for this scenario is `ACCESS DENIED`\)\. These results can be compared to the results obtained from a procmon log of a working installation of the application, which should show as SUCCESS for these operations\.
+ **Enable the compatibility package engine logs** 
  + To enable package engine logging, open the `Compatibility.Package.Engine.clc` file in a text editor, such as Notepad \+\+\.
  + By default, the first line will contain the following: `<AAV PackageId=“PackageID”>`\. For example, `AAV PackageId="SQL2005EXPSP4_8959">`\.
  + Add the string `Log=“on”` to the line so that it reads `<AAV PackageId=“PackageID” Log=“on”>`\. For example, `AAV PackageId="SQL2005EXPSP4_8959" Log="on">`\.
