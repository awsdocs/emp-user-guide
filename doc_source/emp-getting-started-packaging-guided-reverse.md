# How to package an application when installation media is not available \(Guided Reverse Packaging\)<a name="emp-getting-started-packaging-guided-reverse"></a>

You can package your applications running on end\-of\-support Windows servers with guided assistance using Guided Reverse Packaging \(GRP\)\. GRP automates much of the process of creating a compatibility package using the EMP package builder\. It also provides automated dependency recommendations to reduce the amount of manual discovery required to perform the packaging process\. The output of the GRP process is an EMP package, which can run on the latest version of Windows Server\.

**Topics**
+ [Prerequisites for Guided Reverse Packaging](#emp-guided-reverse-prerequisites)
+ [How Guided Reverse Packaging works](#emp-guided-reverse-how-it-works)
+ [EMP Compatibility Package Builder GRP tabs and controls](#emp-guided-reverse-tabs-controls)
+ [Get started with Guided Reverse Packaging](#emp-guided-reverse-get-started)
+ [Use GRP to monitor unspecified processes](#emp-guided-reverse-monitor-processes-other)
+ [Managing advanced configurations](#emp-guided-reverse-managing-advanced)

## Prerequisites for Guided Reverse Packaging<a name="emp-guided-reverse-prerequisites"></a>

In order to use Guided Reverse Packaging \(GRP\), the following prerequisites must be met:
+ You must be able to access the source server with the application you want to package\. The server can be on\-premises or in the cloud\.
+ You must install AWS EMP Compatibility Package Builder using Microsoft Installer \(MSI\)\. For more information, see [ Install AWS EMP Compatibility Package Builder ](emp-install-compatibility-package-builder.md)\.

For limits and OS requirements to use AWS EMP Compatibility Package Builder, see [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server limitations ](emp-limitations.md) and [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server System requirements](emp-supported-os.md)\.

## How Guided Reverse Packaging works<a name="emp-guided-reverse-how-it-works"></a>

The following high\-level steps occur when EMP Guided Reverse Packaging \(GRP\) is performed:

1. **Discovery** — you provide entry points to the application, such as files and folders\.

1. **Dependency discovery**
   + **Static analysis**: GRP analyzes the file and folders that are provided, and captures the relevant dependencies of your application based on data such as dependent components, keywords, timestamps, and application paths\. You can verify these dependencies using GRP by optionally removing dependencies that are not related to the applications\.
   + **Runtime analysis**: You can start and stop runtime analysis in order to monitor the file and registry activity of running processes on the source server\. The runtime analysis integration with GRP is supported only on Windows Server 2008 and later operating systems\. To perform runtime analysis on Windows Server 2003 servers, see [Get started with Guided Reverse Packaging](#emp-guided-reverse-get-started)\.

1. **Package finalization** — you create an EMP compatibility package of the application\.

## EMP Compatibility Package Builder GRP tabs and controls<a name="emp-guided-reverse-tabs-controls"></a>

This section describes the functions of the GRP tabs and controls\.

**GRP tabs**
+ **Selected files** — permits you to select files and folders to include in the package\.
+ **Discovered files** — displays files and folders identified by the discovery process\. You can use the interface to remove any unwanted files and folders by choosing **Exclude** next to the item that you want to remove\. Excluded items are displayed in the **Excluded files** section\. To include an item that was previously excluded, next to the item, choose **Include**\.
+ **COM** — displays discovered COM components\.
+ **Services** — displays discovered services\.
+ **Registry** — displays discovered registry keys\. You can manually add or remove registry keys, if required\. To add a new registry key, enter the path to the key that you want to add, and choose **Add new key**\. The following formats for adding new keys are supported: 
  + `HKCU`
  + `HKLM`
  + `HKEY_CURRENT_USER`
  + `HKEY_LOCAL_MACHINE`

  GRP validates the existence of the key on the source server before adding it\. To delete a key, next to the registry key you want to remove, choose **Exclude**\. The key is moved to the **Excluded keys** section\. To include a key that was previously excluded, choose **Include** next to the key\.

**GRP controls**
+ **Change folder** — allows you to select a different project folder than what you initially specified\. You can switch between different GRP project folders without having to close and reopen GRP each time a different package requires updating\.
+ **Runtime analysis** — allows you to start and stop runtime analysis, so that you can monitor the file and registry activity of running processes on the source server\. The runtime analysis integration with GRP is supported only on Windows Server 2008 and later operating systems\. To perform runtime analysis on Windows Server 2003 servers, see [Get started with Guided Reverse Packaging](#emp-guided-reverse-get-started)\.
+ **Discover dependencies** — allows you to perform GRP discovery with the data collected form the static and runtime analyses\.
+ **Create package** — allows you to create an EMP package when GRP discovery is complete\.

## Get started with Guided Reverse Packaging<a name="emp-guided-reverse-get-started"></a>

**Topics**

1. Download the AWS EMP Compatibility Package Builder to your source server from the [EMP Tooling for End\-of\-Support Migration Program for Windows Server](https://pages.awscloud.com/GLOBAL_PM_LN_emp-tool-registration.html) page\.

1. Run the MSI Installer to install the AWS EMP Compatibility Package Builder on the source server\. If you have already installed a previous version of EMP, you can automatically upgrade using the latest MSI\. For version history, see [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server version history](emp-versions.md)\.

1. Double\-click on the Guided Reverse Packaging \(GRP\) shortcut located on the Windows desktop or Windows Start menu\.

1. Choose an empty folder to start a new packaging project and choose **OK**\. The EMP package will be created in this folder\. 

1. You are taken to the **Selected files** tab, where you have access to other GRP tabs and controls located at the top of the page\. For more information about each tab and control, see [EMP Compatibility Package Builder GRP tabs and controls](#emp-guided-reverse-tabs-controls)\.

1. On the **Selected files** tab, choose:
   + **Add folder** to select the folder where the application is located/installed\. 
   + **Add files** to add any additional files that have not been added with **Add folder**\.

   GRP uses this data to scan the server for application dependencies\. The quality of the dependency discovery is greater when more data is provided at this stage\.

   If you are packaging an application for which you are initially unaware of the required application folders, we recommend that you perform a manual pre\-discovery step, whereby you analyze other entry points of the application to trace back the required application installation directory\. For example, if you know which application services are running in Windows services, check the properties of the application services that contain the application installation folders and files required by GRP\.

1. 

   1. **Windows Server 2008 and later** — Choose **Runtime analysis** to perform typical workflows for the application\. For example, launching the application from the Start menu to perform workflows, and restarting any Windows services\. When you have completed your performance of typical workflows, choose **Runtime analysis** to stop monitoring\. 
**Note**  
To capture high\-quality application runtime dependencies, we recommend that you use the application as it is typically used in your environment\. This includes running through common workflows performed by your users, and any workflows performed at the end of the month or quarter, such as reporting\. This ensures that relevant runtime dependencies are captured in the GRP discovery and packaging cycle\. If runtime analysis misses one or more dependencies because of not being able to perform a unique workflow, you may have to rerun the GRP process\.

   1. **Windows Server 2003** — GRP runtime analysis capability is not supported on Windows Server 2003\. To capture application runtime dependencies on Windows Server 2003, perform the process described in [Log runtime dependencies using Process Monitor](emp-procmon-log-runtime-dependencies.md) \. The outcome of performing this process is one or more CSV\-formatted process monitor log files\. To import the log file data into GRP, add the log files to a separate `Procmon` folder within your project folder \(the one you use to launch GRP\)\. For example, if `C:\GRP` is the project folder location, then you should add your Procmon CSV files to `C:\GRP\Procmon`\.

      Multiple CSV files can be added to the `Procmon` folder\. The GRP discover detection process, explained in the next step, processes the runtime data captured in these logs and displays them in the **Discovered Files** tab\. The **Discovery approach** will be labelled **Procmon**\.

1. **Discover dependencies** will show an amber warning\. Choose **Discover dependencies** to allow GRP to perform the automated discovery process\. A progress bar displays the number of steps completed out of total number of steps in the discovery process\. The amber warning clears when detection completes\.

1. \(Optional\) Verify the data captures in the **Discovered files**, **COM**, **Services**, and **Registry** tabs\. Add or remove any required additional files or registry keys\.
**Note**  
If changes are made to the detected dependencies, then **Discover dependencies** will show an amber warning\. Rerun **Discover dependencies** until the warning is cleared before you move to the next step\. This ensures that your package is created with up\-to\-date discovery data\.

1. Choose **Create package** when you are satisfied with the list of included dependencies\. Enter an application name and ID for the compatibility package, and choose **Package app**\. GRP proceeds to create a package for the application\.

1. When the package creation is complete, choose **Open package** to navigate to the location of the newly created EMP package\.

## Use GRP to monitor unspecified processes<a name="emp-guided-reverse-monitor-processes-other"></a>

GRP only monitors processes that it discovers in the user **Selected Files**\. To discover dependencies for processes that are not provided in these files, navigate to the `GRPRules.json` file in the GRP installation directory, and provide the AdditionalRuntimeProcessName or AdditionalRuntimeProcessIds of the processes to include in the capture\.

Discovering dependencies of unspecified processes can be useful when packaging IIS\-based applications, where it is necessary to monitor the IIS worker process \(w3wp\.exe\)\.

**Process ID example**

```
“AdditionalRuntimeProcessNames”: [],
“AdditionalRuntimeProcessIds”: [1416]
```

**Process Name example**

```
“AdditionalRuntimeProcessNames”: [“w3wp.exe“, ”program.exe“],
“AdditionalRuntimeProcessIds”: []
```

**Note**  
This process applies for both runtime analysis captured as part of the GRP runtime analysis process, as well as for data captured using Process Monitor\.

## Managing advanced configurations<a name="emp-guided-reverse-managing-advanced"></a>

Guided Reverse Packaging \(GRP\) does not support automatic discovery and packaging for applications that use local users and groups/NTFS permissions\. You can migrate applications that use local users and groups/NTFS permissions by using the `DeploymentScript.xml` program task\. For local users and groups, identify the local users and groups to be migrated from the source server\. Note the users added to these groups\. Construct a script that creates these local users and groups on the target Windows server\. Configure the `DeploymentScript.xml` file program task in the package to run this script during package deployment\. For an example configuration, see [Managing EMP custom configurations](emp-custom-configurations.md)\. 

Similar to the process for migrating local users and groups, for NTFS permissions, create a script that migrates the required NTFS permissions, and configure the `DeploymentScript.xml` file to run this script during the package deployment\.

To manually add side\-by\-side assemblies required by the application, see [Add side\-by\-side \(SXS\) assemblies to an EMP compatibility package](emp-sxs-assemblies.md)\.

To package an IIS\-based application, see [Package an IIS\-based application](emp-iis-packaging.md)\.