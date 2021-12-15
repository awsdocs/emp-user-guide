# How to package an application when installation media is not available \(reverse packaging\)<a name="emp-getting-started-packaging-no-media"></a>

**Topics**
+ [Prerequisites](#emp-getting-started-packaging-no-media-prereqs)
+ [Use Process Monitor for discovery](#emp-getting-started-packaging-no-media-discovery)
+ [Filter](#emp-getting-started-packaging-no-media-filter)
+ [Choose processes](#emp-getting-started-packaging-no-media-choose-processes)
+ [Capture](#emp-getting-started-packaging-no-media-capture)
+ [Create the export bundle](#emp-getting-started-packaging-no-media-export)
+ [Create the new package](#emp-getting-started-packaging-no-media-create-package)
+ [Package applications on multiple drives](#emp-getting-started-packaging-no-media-multiple-drives)
+ [Integrate COM\+ applications](#emp-reverse-packaging-com)

## Prerequisites for reverse packaging<a name="emp-getting-started-packaging-no-media-prereqs"></a>

The following prerequisites must be met to successfully package an application when the installation media is not available\.

### Requirements<a name="emp-getting-started-packaging-no-media-prereqs-requirements"></a>

To successfully migrate server applications when the installation media is unavailable, you must have access to a virtual server or instance with a working version of the application\. 

### Software prerequisites<a name="emp-getting-started-packaging-no-media-prereqs-software"></a>

The following software is required to package an application without the installation media\.
+ **Windows Sysinternals Process Monitor \(Procmon\.exe\)**\. Used to trace files, processes, and registry keys of a running application\. Copy the `procmon.exe` tool to a folder on the instance\. In the packaging procedure described in this topic, we copy it to the `ReversePackagingTools` folder of the EMP Compatibility Package Builder installation \.
+ **The EMP package builder**\. To install the EMP Compatibility Package Builder, see [ Install AWS EMP Compatibility Package Builder ](emp-install-compatibility-package-builder.md)\. You can find the reverse packaging tools in the **Tools** folder of the installation\. 
  + On a 32\-bit instance, the default installation folder path is `C:\Program Files\AWS\EMP`\.
  + On a 64\-bit instance, the default installation folder path is `C:\Program Files (x86)\AWS\EMP`\.
+ A **text editor** with syntax highlighting that supports JSON and XML\. [Notepad\+\+](https://notepad-plus-plus.org/) is one free option\.

The next phase of this process is to use Process Monitor to discover the files and registry keys that are used by the application on the source machine\. 

To optimize the setup of Process Monitor for reverse packaging, see [Optimize Process Monitor for Reverse Packaging](emp-procmon.md)\.

## Use Process Monitor to discover the files and registry keys used by the application<a name="emp-getting-started-packaging-no-media-discovery"></a>

The next phase of the process is to use Process Monitor to discover the files and registry keys that are used by the application on the source machine\.

To set up Process Monitor for application discovery, see [Optimize Process Monitor for Reverse Packaging](emp-procmon.md)\.

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

After saving the capture, you must choose which processes to save\. Choose only the processes required by the application to run successfully\.

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

After creating the log file, you must run the EMP reverse packaging tools to process the \.CSV capture file created by Process Monitor\. The following two tools are used in this process:
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

1. Navigate to Windows Explorer and verify that the \.JSON file has been created and that its size is greater than `0`\.

1. Edit the \.JSON file in a text editor, such as Notepad\+\+\.

1. Remove the lines where the path name is the same because a folder includes any subfolders and contents\.   
![\[Lines showing the same path.\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-same-path.png)

   In this example, the files listed in the selected area of the \.JSON file can be updated to `"%ProgramFiles\\Microsoft SQL Server"`, because it includes the folders and contents of this folder\.

1. Remove other items that are not relevant to the application and change entries, such as hard\-coded drive letters, to variables\.

1. Save the file with a different file name, such as `FilteredSource.json`\. This allows you to revert to the original file if you encounter problems later in the packaging process\.

## Create the export bundle of the files and registry keys required for the application<a name="emp-getting-started-packaging-no-media-export"></a>

After you edit and save the filtered source \.JSON file, you must run the `ExportFromSystem` EMP tool\. This tool reads the \.JSON file and compiles copies of files and extracts copies of registry keys required to run the application\. It adds them to the `Files` and `Registry` folders in the `ReversePackagingTools` folder\. When the export is complete, these folders and their contents are compressed into a file called `PackageSource.zip`\.

**Note**  
Before you run this command, verify that all application services and processes are stopped so that all of the required resources are successfully exported from the system\. Any processes and services that are in use will prevent a successful export\.

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

1. Open Windows Explorer and verify that the `PackageSource.zip` has been created and that its size is greater than `0`\. When the `PackageSource.zip` is created, you can use this to create an EMP package for the application\.

## Create the new packaged version of the application<a name="emp-getting-started-packaging-no-media-create-package"></a>

The final step in the reverse packaging process is to create a new packaged version of the application\.

To package an application, the virtual machine on which the application is to be packaged must be running the same version and service packs as the source machine of the application\. 

You must first copy the `PackageSource.zip` that you previously created to a new virtual machine\.

1. Log on to the new virtual machine\.

1. Copy and extract the `PackageSource.zip` to an empty folder\.

   The contents of the `PackageSource.zip` should be the same as the `C:\Program Files\AWS\EMP\Tools\ReversePackagingTools` folder on the source machine\.

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

## Package applications on multiple drives<a name="emp-getting-started-packaging-no-media-multiple-drives"></a>

This section shows the additional configuration required to capture applications that are set up on multiple drives\. For this example, a typical install of SQL Server 2000 is installed on the `C` drive, and the data is stored on the `D` drive\.

**Application directories**
+ **Data directory** — `D:\Program Files\Microsoft SQL Server`
+ **Application directory** — `C:\Program Files\Microsoft SQL Server`

The Process Monitor backing file should be used if process monitoring will take several minutes or hours\. Create this on the non\-system drive with additional filters\.

**`Example: package applications on multiple drives for SQL Server 2000`**

1. Expand `PackageSource.zip` to the `C` drive\.

1. Run the `DeployToSystem` command from the `C` drive location\. File copy errors may occur if you run this command from the `D` drive\.

1. Package files in the `C` drive may contain references to `D` drive locations\. These locations must be redirected in the `AppRegistry.xml` file\.

   ```
   <Write>
       <KeyName>HKEY_CURRENT_USER\Software\AWSEMP\Compatibility.Package|%GUID%\HKLM\SOFTWARE\Microsoft\MSSQLServer\Replication</KeyName>
       <ValueName>WorkingDirectory</ValueName>
       <Value ValueType="String">D:\ProgramFiles\Microsoft SQL Server\MSSQL\REPLDATA</Value>
   </Write>
   ```

   Many applications also have an internal configuration file that contains important file or directory paths\. For example, SQL 2000 includes a `sqlsunin.ini` file with important file paths\. If redirection is not implemented in this file for calls to the `D` drive file locations, the SQL Server fails to start\.

   The procmon and compatibility engine logs will show several `non-redirected` and `path not found` errors, especially in the `ERRORLOG` file\.

1. Add the following folder match redirection to `Redirections.xml` to redirect calls from the `D` drive to the package\.

   ```
   <FolderMatch>
      <From>D:\</From>
      <To>ProgData</To>
   </FolderMatch>
   ```

## Integrate COM\+ applications into EMP packages<a name="emp-reverse-packaging-com"></a>

This topic contains information for scripted integration of COM \+applications into EMP packages\. This includes how to detect whether an application includes COM\+ applications and how to include COM\+ applications in EMP packages\.

**Topics**
+ [Detect COM\+ applications](#emp-reverse-packaging-com-detect)
+ [Include COM\+ applications](#emp-reverse-packaging-com-include)

### Detect whether an application includes COM\+applications<a name="emp-reverse-packaging-com-detect"></a>

**Detect COM\+ applications**

1. Open the Component Services console using one of the following methods\.
   + Run `dcomcnfg` in the command line or using PowerShell and expand Component Services\.
   + Open Component Services from the **Start** menu: **Start**>**All Programs**>**Administrative Tools**>**Component Services**\.
   + Enter `Component Services` in the **Search** box\.

1. Expand Component Services to list the COM\+ applications\. The following are the default COM\+ applications that are included in Windows Server 2003 R2 and 2008 R2, with IIS and Application Server roles enabled\. \.NET Utilities is included only in Windows Server 2003 R2\. COM\+ Utilities \(32\-bit\) is only included on Windows Server 2008 R2\. The rest of the applications are included on both operating system versions\. 
   + \.NET Utilities \(Windows Server 2003 R2\)
   + COM\+ Explorer
   + COM\+ QC Dead Letter Queue Listener
   + COM\+ Utilities
   + COM\+ Utilities 32\-bit \(Windows Server 2008 R2\)
   + System Application

   The names of the COM\+applications typically indicate to which applications they belong\.

### Include COM\+ applications in an EMP package<a name="emp-reverse-packaging-com-include"></a>

To include COM\+ applications in EMP packages, perform an EMP package build using the standard or reverse packaging models\. In this example, the Source Server is the server instance upon which standard packaging was performed, or upon which the process monitoring was performed if the package is built using reverse packaging\.

**Export the COM\+ applications from the source server**

1. Open the Component Services console using one of the following methods\.
   + Run `dcomcnfg` in the command line or using PowerShell and expand Component Services\.
   + Open Component Services from the **Start** menu: **Start**>**All Programs**>**Administrative Tools**>**Component Services**\.
   + Enter `Component Services` in the **Search** box\.

1. Expand COM\+ Applications and identify the COM\+ applications that are installed according to the specified applications\.

1. Right\-click on the first COM\+ application that you want to export to open the context menu, and select **Properties**\.

1. Choose the **Activation** tab to display the **Activation type** details\.

1. Change the activation type to **Server application**\. Choose **OK** on the two warning messages, and then choose the **Advanced** tab\.

1. Under **Debugging**, select **Launch in debugger** so that you can edit the **Debugger path**\. The default debugger path is `C:\Windows\system32\dllhost.exe /ProcessID:{GUID}`\. For example, `C:\Windows\system32\dllhost.exe /ProcessID:{0481F901-E8DC-446C-B82F-7746E380214D}`\.

1. Add the following string to the path, where `<DeployedPackagePath>` is the path to the deployed EMP package \(`%DefaultDir%`\)\. Ensure that there is one space character between this string and the default path:

   ```
   "<DeployedPackagePath>\Compatibility.Package.Engine.exe" /f
   ```

   The debugger path should be set to:

   ```
   "<DeployedPackagePath>\Compatibility.Package.Engine.exe" /f C:\Windows\system32\dllhost.exe /ProcessID:{GUID}
   ```

   For example:

   ```
   "C:\ProgramData\EMP\SQL2005STDSP4_7807\Compatibility.Package.Engine.exe" /f C:\Windows\system32\dllhost.exe /ProcessID:{0481F901-E8DC-446C-B82F-7746E380214D}
   ```

1. Return to the **Activation** tab and set the **Activation type** back to **Library application**\. Choose **Apply** and **OK** to close the COM\+ Properties window\.

1. Repeat steps three through eight for all of the other COM\+ applications you discovered\.

1. Right\-click on the first COM\+ application top open the context menu and select **Export**\.

1. Choose **Next** on the Export Wizard\.

1. Enter the path to the exported MSI file and select **Export user identities with roles**\. Choose **Next**\.

1. Choose **Finish** to complete the export\.

1. The exported COM\+ application MSI and `.cab` files are exported to the specified location\.

1. Repeat steps ten through fourteen for all of the COM\+ applications you discovered\.

1. Copy all of the exported COM\+ applications into the root folder of the EMP package\.

**Discover missing dependent files**

1. On a clean instance running the same operating system as the source server, verify that the required IIS and Application Server roles are enabled\. 

1. Deploy the EMP package\. Do not install the COM\+ export at this time\.

   The exported COM\+ applications in the MSI file often do not include all of the dependencies required to register the COM\+ applications on a new server\. In this case, the dependent libraries would have been captured into the EMP package\. However, when the COM\+ MSI installation runs, the installations fails because the libraries cannot be found\. 

1. Open \(double\-click\) the first COM\+ MSI to start the installation\. If dependencies are not missing and the installations successfully completes, the COM\+ application should appear in the Component Services\. Run the other COM\+ MSI files\. If all of the COM\+ MSI installations complete successfully, then proceed to the next procedure \(Integrate the COM\+ application installation into the package deployment\)\. If any installations fail with the error message `Error registering COM+ Application. Contact your support personnel for more information`, proceed with the next steps to discover and add the missing libraries\.

1. Launch SysInternals Process Monitor \(procmon\), clear the procmon window, start monitoring, and attempt the first failed MSI installation again\.

1. Stop the monitoring when the error appears\.

1. Include a filter with a path that contains `C:\Program Files\COMPlus Applications` \(applicable to 32\-bit COM\+ applications installed on Windows Server 2003 and 64\-bit COM\+ applications installed on Windows Server 2008 R2\)\. Missing DLLs and possibly TLB files are displayed\. If you are running Windows Server 2008 R2 and no missing libraries are displayed, modify your filter to include `C:\Program Files (x86)\COMPlus Applications` to discover any 32\-bit COM\+ libraries required by your application\.

1. Search the ProgData directory for the missing files\. When you have found them, create the native directory `C:\Program Files\COMPlus Applications\GUID` and copy the files from their package location into the GUID folder\.

1. Clear the procmon screen and attempt the installation again\. If it completes, then all of the dependencies have been found\. If it does not complete, repeat step 7 to discover the missing files\. 

1. Repeat this process until all of the COM\+ applications have been successfully installed\. You may have to modify your filter to contain `C:\Program Files (x86)\COMPlus Applications` to pick up any missing 32\-bit COM\+ application libraries\. The COMPlus Applications folders should now include all of the libraries required to register all of the COM\+ applications\.

1. Copy the COMPlus Applications folder from the ` C:\Program Files` into the corresponding location in the EMP package \(`ProgData\Program Files`\)\. Repeat the same process for `C:\Program Files (x86)` if you have discovered any 32\-bit COM\+ libraries on a 64\-bit machine\.

**Integrate the COM\+ application installation into the package deployment**  
When your COM\+ applications are in the package root folder and, if there were any missing COM\+ registration dependencies, the discovered libraries in the COMPlus Applications folders are in the package, add the COM\+ application installation to the deployment using the `DeploymentScript.xml` feature\. The following example tasks are required to add the COM\+ application installation to the deployment\.

The first tasks copy the COMPlus Applications folders into their corresponding native locations\. This is required only if there are any missing COM\+ registration dependencies\. Add another task to copy the `ProgData\Program Files(x86)\COMPlus Applications` if you added this to your package\. The second task, or set of tasks, runs the MSI installations\.

```
<Install>
  <Programs>
    <Program Order="0" PreInstall="true">
      <ProcessWindowStyle>Normal</ProcessWindowStyle>
      <Path>%SystemX86%\CMD.exe</Path>
      <Args>/c XCOPY /E /H /I /R /Q /Y "C:\ProgramData\EMP\SQL2005STDSP4_7807\ProgData\Program Files\COMPlus Applications" "C:\Program Files\COMPlus Applications"</Args>
      <WaitCondition TimeoutInSeconds="0">Exit</WaitCondition>
    </Program>
    <Program Order="1" PostInstall="true">
      <ProcessWindowStyle>Hidden</ProcessWindowStyle>
      <Path>%SystemX86%\msiexec.exe</Path>
      <Args>/i "%DefaultDir%\Microsoft.SqlServer.MSMQTask.MSI" /L*v "%DefaultDir%\Microsoft.SqlServer.MSMQTask.MSIInstallLog.txt</Args>
      <WaitCondition TimeoutInSeconds="0">Exit</WaitCondition>
    </Program>
  </Programs>
</Install>
```

Your deployed package can now be used as a source package for a successful deployment of your package, which includes COM\+ applications\. To test the package, copy it to another server instance\. The next time the package is deployed, it should automatically install the COM\+ applications\. The debugger setting in the COM\+ applications allows them to be virtualized by the package engine\.