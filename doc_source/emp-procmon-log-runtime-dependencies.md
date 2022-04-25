# Log runtime dependencies using Process Monitor<a name="emp-procmon-log-runtime-dependencies"></a>

The following software is required to capture and produce runtime analysis logs to use with [Guided Reverse Packaging \(GRP\)](emp-getting-started-packaging-guided-reverse.md)\.
+ **Windows Sysinternals Process Monitor \(Procmon\.exe\)** — Process Monitor is an advanced monitoring tool for Windows that captures real\-time file system, registry, process, and thread activity\. Process Monitor \(Procmon\) can be used to log application runtime dependencies\. You can use the output log with Guided Reverse Packaging to capture and package the runtime dependencies for your application\.

  You can [download the latest version of Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) from Microsoft\. 
**Note**  
The latest version of Procmon may not function on legacy Windows operating system versions\. For legacy Windows operating systems, access to an earlier, supported version may be required to log runtime dependencies\.
+ A text editor with syntax highlighting functionality, which supports JSON and XML\. [Notepad\+\+](https://notepad-plus-plus.org/downloads/) is a no\-cost option\.

**Topics**
+ [Discover](#emp-procmon-log-runtime-dependencies-discovery)
+ [Filter](#emp-procmon-log-runtime-dependencies-filter)
+ [Choose processes](#emp-procmon-log-runtime-dependencies-choose-processes)

## Discover the files and registry keys used by the application<a name="emp-procmon-log-runtime-dependencies-discovery"></a>

The first step of logging runtime dependencies with Process Monitor is to discover the files and registry keys that are used by the application on the source machine\.

To optimize the setup of Process Monitor for GRP, see [Optimize Process Monitor for Guided Reverse Packaging](emp-procmon.md)\.

### Prepare to discover the application<a name="emp-procmon-log-runtime-dependencies-discovery-prepare"></a>

Complete the following steps to prepare for discovery\.

1. To ensure the quality of the capture, verify that no applications or processes other than those required by Windows are running\.

1. Launch `procmon.exe`\.

1. On the **Process Monitor License Agreement** choose **Agree** to load Process Monitor\.

1. Enable advanced output by selecting **Enable Advanced Output** from the dropdown menu\. This ensures that Process Monitor captures everything that occurs on your machine\.

### Discover the files and registry keys used by your application<a name="emp-procmon-log-runtime-dependencies-discovery-steps"></a>

After you have installed and configured Process Monitor, perform the following steps to discover the application:

1. Load the application that you want to discover\.

1. Use the application to perform typical operations\. To perform a high\-quality discovery of the application files and registry keys, use the common workflows performed by your users\. Be sure to include any workflows performed at the end of the month or quarter, such as reporting\. If you load and close the application without performing common operations, the package will likely fail during user acceptance testing\. We recommend that you use the application as it is typically used in your production environment, and for a longer duration\. This prevents the necessity of repeating the discovery process due to missed files and registry keys\. If you are not sure which workflows are relevant, ask an end user to perform their workflows on the application\.

1. Close the application after you have used all of the features you want to discover\.

1. Return to the Process Monitor application\.

1. On the Process Monitor toolbar, choose **Capture**\. The Process Monitor dialog box will open with the following message: `Disconnecting from Event Tracking for Windows (ETW). This can take up to a minute.` When the disconnection is complete the **Capture** icon will show as crossed out with a red line\.

### <a name="w22aac13c15b9c13"></a>

## Filter the capture<a name="emp-procmon-log-runtime-dependencies-filter"></a>

After you create the capture, you must filter the output and determine which items to safely discard\.

To filter the capture, perform the following steps:

1.  On the Process Monitor toolbar, choose the **Show Process and Thread Activity** icon to clear the selection\. This removes these operations from the Process Monitor trace because they are not required for this process\.

1. On the toolbar, choose the **Show Network Activity** icon to clear the selection\. This removes these operations from the Process Monitor trace because they are not required for this process\.

1. On the toolbar, choose the **Save** icon\.

1. On the **Save to File** pop\-up, verify that both **Events displayed using current filter** and **Also include profiling events** are selected\. Choose **OK**\.
**Note**  
Later in this procedure, we will save only processes that we believe should be included in the capture as the basis for capturing and creating the packaged version of the application\. The reason we save the capture at this point in the process is so that we have the option to reload this version if we later discover that we have left something out of the final capture\. This prevents the necessity of repeating the capture from scratch when we omit something in error\.

1. Enter `CTRL+T` to display the process tree\. Or, select **Process Tree** from the **Tools** menu\. The process tree displays all of the processes captured by Process Monitor\.

## Choose which processes to save in the capture<a name="emp-procmon-log-runtime-dependencies-choose-processes"></a>

After you save the capture, you must choose which processes to save\. Choose only the processes required to run the application successfully\.

To save processes in the capture, perform the following steps:

1. Scroll through the Process Tree until you locate the executable used to load the application\.

1. Choose the executable\.

1. Choose **Include Process** to save this process in the final capture\.

1. Review the Process Tree for additional processes to include in the final capture\. Choose **Include Process** or **Include Subtree**, as required\.

1. After you have reviewed the Process Tree, choose **Close**\.

1. On the toolbar, choose **Save**\.

1. On the **Save to File** pop\-up, verify that both **Events displayed using current filter** and **Also include profiling events** are selected\.

1. Under **Format** select **Comma\-Separated Values \(CSV\)**, and then choose **OK**\.

1. Open Windows Explorer\. Verify that the log file has been created in the \.csv format and that the file size is greater than `0`\.

1. Close Process Monitor\.

For more information about how to use the CSV files generated by the packaging process, see [How to package an application when installation media is not available \(Guided Reverse Packaging\)](emp-getting-started-packaging-guided-reverse.md)\.