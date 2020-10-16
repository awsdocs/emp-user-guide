# Edit, upgrade, and maintain an EMP package<a name="emp-edit-upgrade-maintain"></a>

 The EMP Package Editor is a tool that is used to modify existing EMP compatibility packages\. You can use the Package Editor to apply upgrades, security updates, hot fixes and service packs to the packaged application\. You can also use the Editor to maintain the EMP components\. The editor supports reboots so that you can apply an application update which requires a reboot during installation\. 

**Important**  
You must use the Package Editor to update an EMP package on the same architecture on which the original package was created\. For example, if the package was created on an x86 machine, then the Editor must update the package on an x86 machine\. 

The EMP Package Editor is installed with the EMP Compatibility Package Builder\. A shortcut for the Editor is included in the same menu as the EMP Compatibility Package Builder\.

**Topics**
+ [Edit](#emp-edit)
+ [Upgrade](#emp-upgrade)
+ [Maintain](#emp-update-edit-maintain)

## Edit the application in an EMP package<a name="emp-edit"></a>

Perform the following steps to edit an application in an EMP package\.

1. Verify that you are using the latest version of the EMP Compatibility Package Builder\. For version history, see [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server version history](emp-versions.md)\.

1. Copy the EMP package that you want to edit to the server\.

1. Open the Amazon Web Services folder and launch the Compatibility Package Editor\.

1. On the **Home** tab, choose **Open an existing Compatibility Package**\.

1. Navigate to and choose the package that you want to update\. Then choose **Select Folder**\.

1. To make changes to files and folders, choose the **Files ** tab and navigate to the file or folder you want to edit\. You can review the files and folders added to or removed from the package\. All changes are displayed by default\. The following filters can be applied\. 
   + **New** — Display all files added to the package\.
   + **Modified** — Display all files changed by the update\.
   + **Deleted** — Display all files removed during the application update\.

   When you open the context \(right\-click\) menu on any folder, you can **Add Files**, **Add Folders**, or **Delete** files\. When you right\-click on a file, you can only **Delete** files\.

1. To remove registry keys, open the **Registry** tab, navigate to the registry key that you want to remove, open the context \(right\-click\) menu on the registry key, then choose **Delete**\.

1. When you have finished editing files, folders, or registry keys, choose **Save** to build the changes into an updated package\.

1. When the Package Editor updates a package, it creates a new package folder for the updated package\. It appends the folder name with a version number\. The original packages is left unchanged for future reference\. In addition, the package ID is not changed so that you can use the updated package to update a deployed instance of the original package\.

1. To update a deployed EMP package with an updated version, run the following command on the server running the original deployed package\.

   ```
   <PathtoUpdatedPackage>\Compatibility.Package.Deployment.exe /update
   ```

## Upgrade the application in an EMP package<a name="emp-upgrade"></a>

Perform the following steps to upgrade an application in an EMP package\.

1. Because some patch installers check for the presence of an application before installing updates, verify that you are using the Windows Server operating system version supported by the application and the update, and include the original application installed natively on the server\.

1. Verify that the latest version of the EMP Compatibility Package Builder is installed on the server you want to update\.

1. Copy the EMP package that you want to update to the server\.

1. Open the Amazon Web Services folder and launch the Compatibility Package Editor\.

1. On the **Home** tab, choose **Open an existing Compatibility Package**\.

1. Navigate to and choose the package you want to update, then choose **Select Folder**\.

1. To make changes to files and folders, choose the **Files ** tab and navigate to the file or folder you want to edit\. Choose **Update**\.

1. Install the application patch and make any necessary configuration changes\.

1. When the installation is complete, choose **Next** and the Package Editor merges the updates to the EMP package\.

1. When the merge is complete, all of the updates made to the package by the patch installation can be viewed in the **Files** and **Registry** tabs\. The upper right\-hand corner displays the total number of files \(**All**\), as well as the number of **New**, **Modified**, **Deleted**, and **existing** files\.

1. Choose **Save** to build the updates to a new package\.

## Maintain an EMP package<a name="emp-update-edit-maintain"></a>

When a new version of EMP is released, you can update deployed EMP packages with new binaries\. Perform the following steps to update a deployed EMP package with new EMP binaries\.

1. Verify that the latest version of the EMP Compatibility Packager installed on the server\.

1. Navigate to the Package Builder installation directory\.

1. Navigate to the `Runtime.x64` or the `Runtime.x86` folder, depending on the package architecture\.

1. Copy the files \(or a subset of the files, depending on what requires updating\) in this folder to the folder in the package that requires the update\.
