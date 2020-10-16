# How to package an application when installation media is not available \(reverse packaging\)<a name="emp-getting-started-packaging-no-media"></a>

**Topics**
+ [Prerequisites](#emp-getting-started-packaging-no-media-prereqs)
+ [Use Process Monitor for discovery](#emp-getting-started-packaging-no-media-discovery)
+ [Filter](#emp-getting-started-packaging-no-media-filter)
+ [Choose processes](#emp-getting-started-packaging-no-media-choose-processes)
+ [Capture](#emp-getting-started-packaging-no-media-capture)
+ [Create the export bundle](#emp-getting-started-packaging-no-media-export)
+ [Create the new package](#emp-getting-started-packaging-no-media-create-package)

## Prerequisites for reverse packaging<a name="emp-getting-started-packaging-no-media-prereqs"></a>

The following prerequisites must be met to successfully package an application when the installation media is not available\.

### Requirements<a name="emp-getting-started-packaging-no-media-prereqs-requirements"></a>

To successfully migrate server applications when the installation media is unavailable, you must verify the following requirements\.
+ A virtual machine with a working version of the application\.
+ An identical machine as the original virtual machine running the same operating system and service packs for packaging\.
+ Both machines must meet the [software prerequisites](#emp-getting-started-packaging-no-media-prereqs-software)\.
+ You must be able to stop or disable all non\-essential processes and services on the server before packaging\.

For server applications, we recommend that you create a single package for all components that run on the server role in the application group\. This ensures that processes will not require additional configuration to enable them to communicate with each other when virtualized\. You deploy only a single package when provisioning a new server to perform the required role in the application group\.

### Software<a name="emp-getting-started-packaging-no-media-prereqs-software"></a>

The following software is required to package an application without the installation media\.
+ **Windows Sysinternals Process Monitor \(procman\.exe\)**\. Use to trace files, processes, and registry keys used when an application is running\. 
+ **The EMP package builder**\. The following must be installed to use the package builder:
  + The Windows Imaging Component \(WIC\)
  + Microsoft \.NET Framework version 4\.0 or later
+ **The EMP toolkit**, including the various command line tools, such as `GeneratePackageSource.exe`, `RemvoeKnownFolders.exe`, and `ExportFromSystem.exe`\.
+ A **text editor** with syntax highlighting that supports JSON and XML\. [Notepad\+\+](https://notepad-plus-plus.org/) is one free option\.

To install Process Monitor, `procmon.exe` and the EMP toolkit on the machine that is running the application you want to package, complete the following steps\.

1. Install the EMP Compatibility Package Builder to the default installation folder\. 
   + On a 32\-bit instance, the default installation folder path is: `C:\Program Files\AWS\EMP`\.
   + On a 64\-bit instance, the default installation folder path is: `C:\Program Files (x86)\AWS\EMP`\.

1. Copy the Sysinternals Process Monitor, `procmon.exe` to the folder on the instance\. In the procedure described in this topic, we copy it to `C:\EMP\Tools`\.

### Set up Process Monitor for Reverse Packaging<a name="emp-procmon"></a>

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
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/emp/latest/userguide/emp-getting-started-packaging-no-media.html)

You can further expand the number of exclusion items by performing the following steps\.

1. Run procmon for a limited period of time or without the execution process\.

1. Analyze the capture logs and note any processes that are not related to the application\.

1. Add the unrelated processes as additional exclusion items\.

1. For applications where a complete list of required processes are known, you can start a Process Monitor capture and include only these processes in the capture\. If this method results in missed process applications, the final logs may not contain the required information to complete a working package\.

## Use Process Monitor to discover the files and registry keys used by the application<a name="emp-getting-started-packaging-no-media-discovery"></a>

The next phase of the process is to use Process Monitor to discover the files and registry keys that are used by the application on the source machine\.

To set up Process Monitor for application discovery, see [Set up Process Monitor for Reverse Packaging](#emp-procmon)\.

**Topics**
+ [Prepare to discover the application](#emp-getting-started-packaging-no-media-discovery-prepare)
+ [Discover the files and registry keys used by the application](#emp-getting-started-packaging-no-media-discovery-perform)

### Prepare to discover the application<a name="emp-getting-started-packaging-no-media-discovery-prepare"></a>

To prepare for discovery, complete the following steps\.

1. Verify that no applications or processes other than those required by Windows are running\. This improves the quality of the capture\.

1. Navigate to the `ReversePackagingTools` folder\.

1. Open \(double\-click\) `procmon.exe` to launch Process Monitor\.

1. On the **Process Monitor License Agreement** pop\-up, choose **Agree** to load Process Monitor\.

1. To ensure that Process Monitor captures everything that happens on the machine, you must enable advanced output by selecting **Enable Advanced Output** from the Filter dropdown menu\.

### Discover the files and registry keys used by the application<a name="emp-getting-started-packaging-no-media-discovery-perform"></a>

After Process Monitor is installed and configured, complete the following steps to discover the application:

1. Load the application that you want to discover\.

1. Use the application to perform typical operations\.

   To get a high\-quality discovery of the application files and registry keys, use the common workflows performed by your users and any workflows performed at the end of the month or quarter, such as reporting\. If you load and close the application only, you are more likely to generate a package that fails during user acceptance testing\. We recommend that you use the application as it is typically used in your production environment, and for a longer period of time\. This prevents the necessity of repeating the discovery process because of missed files and registry keys\. 

   If you are not sure which workflows are relevant to the application, ask an end user of the application to perform their workflows on the application\.

1. When you have finished using all of the features that you want to discover, close the application\.

1. Return to the Process Monitor application\.

1. Choose the **Capture** icon \(image of a magnifying glass\) on the Process Monitor toolbar\. The **Process Monitor** dialog box will display, stating `Disconnecting from Event Tracking for Windows (ETW). This can take up to a minute.` When the disconnection completes, the **Capture** icon will appear as crossed out by a red line\.

## Filter the capture<a name="emp-getting-started-packaging-no-media-filter"></a>

After you create the capture, you must filter the output and determine which items to safely discard\.

To filter the capture, perform the following steps:

1. Choose the **Show Process and Thread Activity** icon on the Process Monitor toolbar to clear the selection\. This removes these operations from the Process Monitor trace because they are not required for this process\.

1. Choose the **Show Network Activity** icon on the Process Monitor toolbar to clear the selection\. This removes these operations from the Process Monitor trace because they are not required for this process\.

1. Choose the **Save** icon on the toolbar\.

1. On the **Save to File** pop\-up, verify that both **Events displayed using current filter** and **Also include profiling events** are selected, then choose **OK**\.
**Note**  
Later in this procedure, we will save only the processes that we believe should be included in the capture as the basis for capturing and creating the packaged version of the application\. The reason we save the capture at this point in the overall process is to have the option to reload this version of the capture if we later realized we have omitted something from the final capture\. This prevents the necessity of repeating the capture from scratch if we omitted something in error\. 

1. Display the process tree by entering `CTRL+T` or by selecting **Process Tree** from the **Tools** menu\. The process tree displays, showing all of the processes captured by the Process Monitor\.

## Choose which processes to save in the capture<a name="emp-getting-started-packaging-no-media-choose-processes"></a>

After saving the processes, you must choose which processes to save as the basis for creating the application\. You must also include any external dependencies required by the application\.

**Important**  
Do not proceed with saving the capture before eliminating processes that are not required by the service\. Include only those processes required by the application to run successfully\.

To save processes in the capture, complete the following steps\.

1. Scroll through the Process Tree until you locate the executable used to load the application\.

1. Choose the executable\.

1. Choose **Include Process** to save this process in the final capture\.

1. Review the Process Tree for additional processes to include in the final capture\. Choose **Include Process** or **Include Subtree** as required\.

1. When you are finished reviewing the Process Tree, choose **Close**\.

1. On the toolbar, choose the **Save** icon\.

1. On the **Save to File** pop\-up, verify that both **Events displayed using current filter** and **Also include profiling events** are selected\.

1. Under the **Format** section, select **Comma\-Separated Values \(CSV\)**, and then choose **OK**\.

1. Open Windows Explorer and verify that the log file has been created in the \.CSV format, and that the file size is greater than `0`\.

1. Close the Process Monitor\.

## Capture the files and registries<a name="emp-getting-started-packaging-no-media-capture"></a>

After creating the log file, you must run the EMP tools to process the \.CSV capture file created by the Process Monitor\. The following two tools are used in this process:
+ **GeneratePackageSource**\. This command compiles a list of the files and registry keys accessed by the application from the Process Monitor capture\. These are exported to a \.JSON file one line at a time\.
+ **RemoveKnownFolders**\. This command removes known files, folders, and registry keys that are not required from the Process Monitor capture\. Examples include background Windows services and components that are already present on the target server\.

To process the capture file with EMP tools, complete the following steps\.

1. Open a command prompt as an administrator\.

1. Navigate to the folder that contains the Process Monitor capture files, for example, `C:\Program Files\AWS\EMP\Tools\ReversePackagingTools`\.

1. Enter the following command, where `<procmoncapture.CSV>` is the name of the \.CSV file that you created and `<outputfilename.json>` is the name of output file that you want to create:

   ```
   type <procmoncapture.CSV> | GeneratePackageSource.exe | RemoveKnownFolders.exe > <outputfilename.json>
   ```

   For example:

   ```
   type Logfile.CSV | GeneratePackageSource.exe | RemoveKnownFolders.exe > source.json
   ```
**Note**  
The `GeneratePackageSource` and `RemoveKnownFolders` commands can be run one at a time by first piping the `logfile.CSV` into `GeneratePackageSource.exe`, and then running `RemoveKnownFolders.exe` to generate the `source.json`\. If the `GeneratePackageSource.exe` hangs, the Process Monitor may still have a lock on the CSV file\. If this is the case, resolve by rebooting the machine\.

   When the command completes, the `C:\Program Files\AWS\EMP\Tools\ReversePackagingTools` displays\. Leave the command prompt open because it will be used again later in this process\.

1. Navigate to Windows Explorer and verify that the \.JSON file has been created and that its size is greater than `0`\.

1. Edit the \.JSON file in a text editor, such as Notepad\+\+\.

1. Remove the lines where the path name is the same because a folder includes any subfolders and contents\.   
![\[Lines showing the same path.\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-same-path.png)

   In this example, the \.JSON file can be updated to `"%ProgramFiles\\Microsoft SQL Server"`, which includes the folders and contents of this folder listed previously\.

1. Remove other items that are not relevant to the application and change entries, such as hard\-coded drive letters, to variables\.

1. Save the file with a different file name, such as `FilteredSource.json`\. This allows you to revert to the original file if you encounter problems later in the packaging process\.

## Create the export bundle of the files and registry keys required for the application<a name="emp-getting-started-packaging-no-media-export"></a>

After you edit and save the filtered source \.JSON file, you must run the `ExportFromSystem` EMP tool\. This tool reads the \.JSON file and compiles copies of files and extracts copies of registry keys required to run the application\. It adds them to the `Files` and `Registry` folders in the `ReversePackagingTools` folder\. When the export is complete, these folders and their contents are compressed into a file called `PackageSource.zip`\.

**Note**  
Before you run this command, verify that the SQL Server 2000 application services and processes are stopped so that all of the required resources are successfully exported from the system\. Any processes and services that are in use will prevent a successful export\. To do this, open SQL Server Services Manager from the System tray and stop each of the services in the **Services** list\. When the services are stopped, open the context \(right\-click\) the application in the System tray, and choose **Exit**\.

Follow these steps to run the `ExportFromSystem` EMP tool\.

1. Return to the administrator command prompt that you opened previously\. If you closed it, open a new command prompt as an administrator and navigate to ` C:\Program Files\AWS\EMP\Tools\ReversePackagingTools`\.

1. Enter the following command\. `<filterfile.JSON>` is the name of the \.JSON file that you created when you captured the files and registry keys\. `<logfilename.txt>` is the name of the log file that you want to create to capture the output of running the `ExportFromSystem` command\.

   ```
    type <filterfile.JSON> | ExportFromSystem.exe > <logfilename.txt>
   ```

   For example:

   ```
    type FilteredSource.json | ExportFromSystem.exe > exportfromsystem.txt
   ```

   We recommend that you pipe the output to a file to check for errors\.
**Note**  
It takes a while before the `C:\Program Files\AWS\EMP\Tools\ReversePackagingTools` prompt displays\. This is because of the number of actions being performed by the `ExportFromSystem` tool\.

1. Open Windows Explorer and verify that the `PackageSource.zip` has been created and that its size is greater than `0`\. When the `PackageSource.zip` is created, you can run the application on a new virtual machine\.

## Create the new packaged version of the application<a name="emp-getting-started-packaging-no-media-create-package"></a>

The final steps in the reverse packaging process are to create a new packaged version of the application\.

To package an application, the virtual machine on which the application is to be packaged must be running the same version and service packs as the source machine of the application\. The target virtual machine must not have any IAM policies applied to it\.

You must first copy the `PackageSource.zip` that was created on the source computer to a new virtual machine\.

1. Log on to the new virtual machine\.

1. Within the `C:\\EMP` folder, create a folder with the name of the application to be packaged and add the `PackageSource.zip` to this folder\.

1. Extract the contents of the `PackageSource.zip` to this folder\.

   The contents of the `PackageSource.zip` should be the same as the ` C:\Program Files\AWS\EMP\Tools\ReversePackagingTools` folder on the source machine\.

1. Follow the steps to create a new package using the EMP Compatibility Package Builder\. When **Install Application** is displayed, you can install the application\.

You can now install the application using the EMP `DeploytoSystem` tool, which automates the installation process\. The application must be installed with this tool, and not with the application's native installation system\.

1. As an administrator, open a command prompt\.

1. Go to the folder that contains the files extracted from the `PackageSource.zip`\.

1. Enter the following command, where `<filterfile.JSON>` is the name of the \.JSON file that you previously created of the final list of files and registry keys used by the application, and `<logfilename.txt>` is the name of the log file that you want to create to capture the output of the `DeployToSystem` command\.

   ```
   type <filterfile.JSON> | DeployToSystem.exe > <logfilename.txt>
   ```

   For example:

   ```
   type FilteredSource.json | DeployToSystem.exe > deploytosystem.txt
   ```

   We recommend that you pipe the output to a file to check for errors\.

   It will take a while before your folder is redisplayed\. If you are prompted to reboot the instance, you should do so only after the application is installed\. If you reboot, run the Compatibility Package Builder using the desktop shortcut\.

   When the command prompt is displayed, return to the EMP Compatibility Package Builder to complete the packaging process\.
