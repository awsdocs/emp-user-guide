# Optimize Process Monitor for Guided Reverse Packaging<a name="emp-procmon"></a>

Process Monitor is an advanced monitoring tool for Windows that captures real\-time file system, registry, process, and thread activity\. The first step of the EMP reverse packaging process is to capture a Process Monitor \(procmon\) log of the entire functional running of the application on the source operating system\. The log is used to create an EMP package consisting of all of the required components for the application to successfully function on a modern operating systems after it has been migrated\. An incomplete capture can result in missing application components\.

You can download the latest version of Process Monitor from Microsoft at: [https://docs\.microsoft\.com/en\-us/sysinternals/downloads/procmon](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)\.

Perform the following steps to set up capturing for Process Monitor\. Use these steps only as a guide\. The monitoring process requirements for each application can vary\.

1. Download the tool on the system running the application that you want to discover\.

1. As an administrator, open the command prompt, and launch Process Monitor\.

   ```
    Procmon.exe /AcceptEula /Noconnect
   ```

   `/AcceptEula` automatically accepts the EULA license and bypasses the EULA dialog box\.

   `/Noconnect` This flag prevents Process Monitor from automatically starting log activity\.

1. Configure Process Monitor to save captured logs to a backing file as opposed to virtual memory by navigating to **File**>**Backing Files** and choosing the **Use file name** option\. Select the location and file to which you want to save the backing file\.
**Note**  
If the system used to capture the logs does not have sufficient storage capacity, you can store the data in another location, such as on a different server or external storage device\.

1. Start the capture by choosing **Capture**\. To stop the capture, choose **Capture** again\.

The following optional steps reduce the size of the log file, where possible\. To reduce the size of the log file, we recommend that you run procmon only when running the application windows to reduce capture of background noise and unrelated workflows\. 

1. Verify that the following options are not selected:
   + **Process and Thread Activity**
   + **Network Activity**
   + **Profiling Events**

1. Select **Drop Filtered Events** in the **Filter** menu\. This prevents events that don't meet the filter criteria from being added to the log\.

1. The following table contains common exclusion items related to the operating system that are not required for the application capture\. You can add these exclusions to the Process Monitor application capture exclusion filter\.  
**Exclusions in the `EMP_Procmon_Exclusions.PMF` filter**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/emp/latest/userguide/emp-procmon.html)

You can further expand the number of exclusion items by performing the following steps\.

1. Run procmon for a limited period of time or without the execution process\.

1. Analyze the capture logs and note any processes that are not related to the application\.

1. Add the unrelated processes as additional exclusion items\.

1. For applications where a complete list of required processes are known, you can start a Process Monitor capture and include only these processes in the capture\. If this method results in missed process applications, the final logs may not contain the required information to complete a working package\.