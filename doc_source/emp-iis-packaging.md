# Package an IIS\-based application<a name="emp-iis-packaging"></a>

EMP supports packaging and migrating legacy IIS\-based applications that run on Windows Server 2003, Windows Server 2008, and Windows Server 2008 R2 to the latest, supported versions of Windows Server running on AWS\.

**Topics**
+ [Discovery](#emp-iis-packaging-discovery)
+ [Migrate your IIS\-based application](#emp-iis-packaging-migration)
+ [Troubleshooting packaging an IIS\-based application](#emp-iis-packaging-troubleshooting)

## Discovery<a name="emp-iis-packaging-discovery"></a>

The first step of a migration plan is to identify additional IIS\-based application dependencies that are installed on the same server as the application you are migrating\. For example, an IIS\-based application can be dependent on a third\-party application that generates reports, such as *Crystal Reports*\.

If the dependency information is not available, try the following steps:

1. Navigate to and inspect the configuration files of the web application for dependencies\.

   The following example `web.config` file, found in `C:\inetpub\wwwroot` of an IIS\-based application, shows a dependency on *Crystal Reports* assemblies:  
![\[Contents of example web.config file\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-web-config-example.png)

1. Capture a Process Monitor log file of your application usage\. See [Use Process Monitor to discover the files and registry keys used by the application](emp-getting-started-packaging-no-media.md#emp-getting-started-packaging-no-media-discovery) for the steps to capture a Process Monitor log file\. The captured log file can reveal third\-party application dependencies\.

   The following example Process Monitor log file shows that the sample IIS\-based application depends on a legacy version of *Crystal Reports*\. An IIS worker process called `w2wp.exe` handles the web requests sent to the IIS web server for the configured IIS application pool\.  
![\[Contents of example Process Monitor log file\]](http://docs.aws.amazon.com/emp/latest/userguide/images/process-monitor-example-iis.png)

## Migrate your IIS\-based application<a name="emp-iis-packaging-migration"></a>

There are two stages to migrate an IIS\-based application:

1. **Web application migration** — Migrate the web application configuration from the legacy version of the IIS\-based application on an unsupported Windows Server version to a modern version of IIS on a supported Windows Server version\.

1. **Standard or reverse packaging \(optional\)** — This option applies only when application dependencies are identified during discovery\. Follow either the [standard](emp-getting-started-packaging-media.md) or [reverse](emp-getting-started-packaging-no-media.md) packaging process to capture the application dependencies in an EMP package and link them to the migrated IIS\-based web application running on a modern Windows Server version\.

### Stage 1: Migrate your web application configuration<a name="emp-iis-packaging-migrate-config"></a>

Stage 1 of the IIS\-based application migration process consists of steps to apply on the legacy and target servers\.

#### Legacy server steps<a name="emp-iis-packaging-migration-legacy-server"></a>

Apply the following steps to the legacy \(source\) server that hosts the IIS\-based web application:

1. [Install the EMP Compatibility Package Builder](emp-install-compatibility-package-builder.md) on the legacy server\. The EMP IIS migration tools are located in the `Tools/IISTools` folder within the installation directory \(64\-bit system: `C:\Program Files (x86)\AWS\EMP\` or 32\-bit system: `C:\Program Files\AWS\EMP\`\)\.

1. Launch the **Internet Information Services \(IIS\) Manager** shortcut on the legacy server and, within the **Sites** node, identify the name of the website\(s\) you want to migrate\.

1. Open PowerShell as an administrator and change the directory to the `IISTools` folder\. Then, run the following command:

   ```
   PS C:\> Export-IISWebSiteWithDependentFeatures.ps1 -Name Website1,Website2 -OutputDirectory C:\DestinationFolder
   ```

   `-Name` — specify the name of one or more website\(s\) identified in the **Sites** node\.

   `-OutputDirectory` — specify the folder to which the website contents and configuration will be saved\.

   When you run this command, the installation of [MSDeploy](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) provided with the EMP release and included in the `IISTools` folder will be silently installed if it is not already\.

   When the command completes, a folder is created in the output directory location that you specified\. The folder is called `EMP-IIS`\. In addition, this folder captures the Windows features that are installed on the legacy server\.

   Inspect the output of the `Export[WebsiteName].msdeploy.err` and `ExportGlobalConfig.msdeploy.err` files for runtime errors and remediate as required\. An empty file indicates that no errors were recorded\. 

   Copy the `EMP-IIS` folder to the target server\.

1. Uninstall the EMP Compatibility Package Builder from the source server\.

1. Run the PowerShell script `Uninstall-MSDeploy.ps1` provided in the `MSDeploy` folder to uninstall the Web Deploy application\.

#### Target server steps<a name="emp-iis-packaging-target-server"></a>

Apply the following steps to the target server to which the IIS\-based web application will be migrated\.

1. Install the EMP Compatibility Package Builder on the target server\. The EMP IIS migration tools are located in the `Tools/IISTools` folder within the installation directory \(64\-bit system: `C:\Program Files (x86)\AWS\EMP\` or 32\-bit system: `C:\Program Files\AWS\EMP\`\)\.

1. Open PowerShell as an administrator and change the directory to the `IISTools` folder\. Then, run the following command\. If you are migrating more than one website, run this command for each website you are migrating\.

   ```
   PS C:\>  Import-IISWebSiteWithDependentFeatures.ps1 -Path C:\DestinationFolder\EMP-IIS
   ```

   `-WebSite` — specify the name of one or more website\(s\) identified in the **Sites** node\.

   `-EMPPackagePath` — specify the folder to which the EMP package has been deployed\. For example, `C:\Programdata\EMP\Package0001`\.

   When you run this command, the installation of [MSDeploy](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) provided with the EMP release and included in the `IISTools` folder will be silently installed if it is not already\.

   The command will then install and set up the server with the Windows features identified from the source server\. If a feature that was identified in the source server is deprecated and a replacement feature is not identified, a warning message is displayed\. You can edit the `Config.xml` file located in the root of the `EMP-IIS` folder if a manual change to the list of features to install is required\.

   For Windows Server 2003 applications, a list of Windows features on the legacy server is not identified\. Instead, a default list of Windows features is configured on the target operating system\.

   When the command completes, the web application configuration of the IIS\-based web application is set up on the target server\.

   Inspect the output of the `Import[WebsiteName].msdeploy.err` and `ImportGlobalConfig.msdeploy.err` files for runtime errors and remediate as required\. An empty file indicates that no errors were recorded\.

1. Uninstall the EMP Compatibility Package Builder from the target operating system\. If you discovered application dependencies and are moving to the next stage to capture them, do not uninstall the EMP Compatibility Package Builder\.

### Stage 2: Capture application dependencies \(optional\)<a name="emp-iis-packaging-capture-dependencies"></a>

This procedure is necessary only if you identify application dependencies in the discovery phase\.

1. Follow either the [standard](emp-getting-started-packaging-media.md) or [reverse](emp-getting-started-packaging-no-media.md) packaging process to capture the application dependencies in an EMP package\. Move the package to the target server and [deploy ](emp-deploy.md)the package on the target server with an additional `/DeployAllRegistry` switch\. 

   **Example command**

   ```
   C:\EMP\Package0001\Compatibility.Package.Deployment.Exe /acceptEULA /deploydir "C:\Programdata\EMP" /DeployAllRegistry
   ```

   The `/DeployAllRegistry` switch makes the EMP package accessible at the machine level and ensures that IIS\-based Windows accounts such as IUSR can access the EMP package registry when required\.

1. Open PowerShell as an administrator and navigate to the `IISTools` folder\. Run the following command\.

   ```
   PS C:\> Set-IISEMPConfigurations.ps1 -WebSite WebSite -EMPPackagePath c:\EMPPackageDeployLocation
   ```

   `-WebSite` — specify the name of the website\(s\) identified in the **Sites** node\.

   `-EMPPackagePath` — specify the folder to which the website contents and configuration will be saved\. For example, `C:\Programdata\EMP\Package0001`\. 

   This command will set the necessary IIS\-related integration configurations required for the EMP package\.

1. The IIS\-based web application migration is complete and you can begin user testing\.

## Troubleshooting packaging an IIS\-based application<a name="emp-iis-packaging-troubleshooting"></a>

The following actions can help you troubleshoot issues that can occur when you package an IIS\-based application\.

**Set `Enable 32-bit application` application pool setting to `True`**  
Some applications require the `Enable 32-bit application` application pool to be set to `True` in order to work on a modern operating system\. This is especially true for applications for which this setting is currently set to `True` in the legacy environment, or if the application has been ported from a 32\-bit system\. EMP does not set this option as part of the migration process\.

**Create IIS\-based application migration log files**  
When you run the following PowerShell scripts, import and export log files are created in the `EMP-IIS` folder\.

```
PS C:\> Export-IISWebSiteWithDependentFeatures.ps1
```

```
PS C:\> Import-IISWebSiteWithDependentFeatures.ps1
```

The `.err` files log errors when `.err` commands are run\. The `.out` files create a descriptive log of the running of the command\.

**Sample log files**

Export

```
Export[WebsiteName].msdeploy.err
Export[WebsiteName].msdeploy.out
ExportGlobalConfig.msdeploy.err
ExportGlobalConfig.msdeploy.out
```

Import

```
Import[WebsiteName].msdeploy.err
Import[WebsiteName].msdeploy.out
ImportGlobalConfig.msdeploy.err
ImportGlobalConfig.msdeploy.out
```

**Use the scripts provided in the `IISTools` folder to help troubleshoot errors**  
The PowerShell scripts located in the `IISTools` folder support the `-confirm`, `-whatif`, `-verbose`, and `help` parameters\.
