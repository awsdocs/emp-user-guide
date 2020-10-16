# How to package an application when installation media is available \(standard packaging\)<a name="emp-getting-started-packaging-media"></a>

When installation media is available, an application is packaged with the EMP compatibility package builder\. The package builder uses installation snapshot\-based packaging along with runtime analysis to create compatibility packages for applications\. 

**Topics**
+ [Stage 1: Install capture \(Required\)](#emp-media-install-capture)
+ [Stage 2: Runtime analysis \(Optional\)](#emp-media-runtime-analysis)
+ [Stage 3: Edit package contents \(Optional\)](#emp-media-edit-package)
+ [Stage 4: Package Finalization \(Required\)](#emp-media-package-finalization)

## Stage 1: Install capture \(Required\)<a name="emp-media-install-capture"></a>

1. After you install the tools from the [End\-of\-Support Migration Program for Windows Server](http://aws.amazon.com/emp-windows-server/) product page, launch the package builder from your desktop\.

1. On the **Select Package Folder** page, choose **Select Folder**\. Select a package folder to specify where the package will be created, then choose **OK**\.

1. Choose **Next**\.

1. On the **Start Capture** page, choose **Start Capture** to capture the state of the system before the application is installed\.

1. Choose **Next** when the capture is complete\.

1. Install the application, components, and any required dependencies\.

1. When all of the installations have completed, reboot **ONLY** if required by the application installer\. After the reboot completes, you can restart the package builder from your desktop\.

1. Return to the package builder, and on the **Install Application** page, choose **Next**\.

1. If required, proceed to [Stage 2: Runtime analysis \(Optional\)](#emp-media-runtime-analysis); otherwise, choose **Next** to proceed to [Stage 3: Edit package contents \(Optional\)](#emp-media-edit-package) \.

**Note**  
When it is launched from the desktop shortcut only, runtime analysis reviews file and registry operations \(for example, read, modification, and creation\) performed by processes\. This is not a required step, but can be used to review and include changes to the registry and file system during the first run of the application\. It can also be helpful to review what the application processes do when running under the EMP engine\.

## Stage 2: Runtime analysis \(Optional\)<a name="emp-media-runtime-analysis"></a>

The package builder detects all files, registry keys, and shortcuts created or modified when the application runs for the first time\. Shortcuts are displayed on the **Run Installed Applications** screen\. If no shortcuts are displayed, proceed to [Stage 3: Edit package contents \(Optional\)](#emp-media-edit-package)\.

**Important**  
Do not use any desktop or **Start** menu shortcuts\. Start the application using only the shortcuts displayed in the package builder\. If you use shortcuts other than those displayed in the package builder, configuration changes will not be captured by the package builder\.

The following steps enable application files and registry entries that are created when the application is configured for the first time to be captured in the package\.

1. Start the application by using the shortcut in the package builder\. Do not use shortcuts on the desktop or **Start** menu\.

1. Choose each shortcut to load the shortcuts\.

1. Perform any required configuration changes to ensure that the application is configured for users the first time it starts\. If the packaged application fails during a particular workflow, or an issue is identified during user acceptance testing \(UAT\), try to repeat the workflow during the packaging process\.

1. Close the programs, and choose **Next** to continue\.

1. Choose **Complete Capture** to record the final state of the system\.

1. After the capture successfully completes, the following message displays `Capture completed. Please click “next” to continue`\. Choose **Next**\.

**Important**  
Do not uninstall the application during runtime analysis or the package builder will capture this change and create the package without the application\.

## Stage 3: Edit package contents \(Optional\)<a name="emp-media-edit-package"></a>

Files, registry keys, and redirects can be added to, removed from, or modified in the package depending on what was captured during the install capture and runtime analysis\. 

 The following steps show how to modify the contents of the package\.

1. On the **Captured Files ** page, you can use the left\-hand pane **Files in package** to view or remove files, or to add redirections for files captured in the package by the install capture process\.

1. Navigate to the file or folder, open the context \(right\-click\) menu, then choose **Redirect** or **Remove Item**, as required\. If you choose a folder and want to redirect all subfolders, then choose **Redirect Children**\.

   If you redirect or remove an item, the available options on the context menu changes to **Remove Redirect** or **Add Item**, which allows you to reverse your changes\.

1. To include the files detected by runtime analysis, use the right\-hand pane **Files requested at runtime**

1. Navigate to the file, open the context \(right\-click\) menu, and choose **Include in package**\.

1. Choose **Next**, which displays the **Captured Registry Keys** page\. From this page, you can view and manage registry keys in the same way as files\.

1. Choose **Next** when you have made the required changes for registry keys\.

## Stage 4: Package Finalization \(Required\)<a name="emp-media-package-finalization"></a>

1. On the **Package** page, in the **App Name** box, enter a unique name for the application, which automatically populates the **App ID** box\.

1. Optionally, enter a description for the application in the **Description** box\.

1. From the **Run** drop\-down list, optionally select the executable that is used to load the application\.

1. Choose **Package App** to create the package\.

1. Choose **Next** when the `Press “Next” to continue` message is displayed\.

1. To view the contents of the package, choose **Open Package**\.

1. To make changes, choose **Edit this Package** to modify the contents of the package or to change the name, description, and initial executable in the package\.

1. When you are finished, close the package builder by choosing the **X** in the top right\-hand corner\.
