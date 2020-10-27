# Components of the EMP Compatibility Package Builder<a name="emp-components"></a>

The installation directory of the Compatibility Package Builder includes the following files and folders\.

**Files**
+ **Compatibility\.Package\.Builder\.exe** — the Package Builder program\.
+ **Compatibility\.Package\.Builder\.cfg** — Used to configure packager settings, such as file scan root directory\.
+ **Compatibility\.Package\.Builder\.exe\.config** — Contains packager program settings, such as packager event logging level and dependencies, such as the \.NET runtime version\.
+ **Compatibility\.Package\.Builder\.log** — Logs package builder events during packaging\.
+ **Compatibility\.Package\.CmdLineBuilder\.exe** — The CLI version of the Package Builder\. Uses the `PackageScript.xml` as its response file\. 
+ **Compatibility\.Package\.CmdLineBuilder\.exe\.config** — Includes the CLI program settings, such as CLI event logging level and dependencies, such as the \.NET runtime version\.
+ **EULA\.rtf** and **eula\.base** — The end\-user\-license\-agreement files\. the content of the `.rtf` file is copied into the `.base` file, which is saved as HTML in the package root folder\. 
+ **ExclusionList\.json** — A configuration file that contains the list of files, folders, and registry keys that are ignored during scanning\.
+ **Open Source Licenses\.txt** — Contains the license for the open\-source components in the Package Builder\.

**Folders**
+ **Images** — Contains the graphic files used by the Package Builder application\.
+ **x64 \(Engine Binaries\)** — Contains the EMP runtime files for packages to be deployed on a 64\-bit system\. When packaging is performed on a 64\-bit machine, the contents of this folder are automatically copied into the root folder of the EMP package during the package build\. An EMP package that was built on a 64\-bit machine cannot be run on a 32\-bit machine\.
+ **x86 \(Engine Binaries\)** — Contains the EMP runtime files for packages to be deployed on a 32\-bit system\. When packaging is performed on a 32\-bit machine, the contents of this folder are automatically copied into the root folder of the EMP package during the package build\. An EMP package that was built on a 32\-bit machine will run on a 64\-bit machine, however, we recommend that you use the appropriate runtimes for the destination platform\. This is automatically handled during the deployment process\.
+ **Tools** — Contains three sets of tools that are available to EMP packages\.
  + **DiscoveryTool** — A command line tool that can perform a limited discovery of a server\.

**Commands**
    + `Compatibility.Package.DiscoveryTool.exe -d "<InstallDirectory>` — Writes a list of loaded COM servers and drivers in `<Currentdirectory>\Report.json`\.
    + `Compatibility.Package.DiscoveryTool.exe -l` — Lists features and subfeatures of the tool in the command console\.
  + **Editor** — Used to edit existing packages\. This tool has a shortcut in the **Start** menu, where it goes by **Compatibility Package Editor**\. 
  + **ReversePackagingTools** — A command line tool set used in the reverse packaging process, a method of compatibility packaging used when application installation media is not available\. This tool set is used during the first two stages of reverse packaging:

    1. **Install media reverse engineering** — A reverse engineered installation media, called a package source is generated from a working instance of the application on the server\.

    1. **EMP package build** — The package source installation is then captured on a clean packaging server using the Compatibility Package Builder\. 

**Commands**
    + `type <procmoncapture.CSV> | GeneratePackageSource.exe | RemoveKnownFolders.exe > <outputfilename.json>` — Generates a manifest for the package source from the CSV file using `RemoveKnownFolders.exe` to remove common known system registry keys, files, and folders\.
    + `type <filteredoutputfilename.json> | ExportFromSystem.exe > <logfilename.txt>` — Extracts the listed files, directories, and registry keys from the operating system and compresses them along with the remaining contents of the `ReversePackagingTools` folder into `PackageSource.zip`\. This file is a reverse\-engineered installation media for the application\.
    + `type <filteredoutputfilename.json> | DeployToSystem.exe > <logfilename.txt>` — Installs Package Source during the package build on the packaging server using the package builder\.
