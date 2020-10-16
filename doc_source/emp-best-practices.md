# Best practices for packaging applications with AWS End\-of\-Support Migration Program \(EMP\) for Windows Server<a name="emp-best-practices"></a>

Use the following best practices when packaging applications using EMP for successful packaging and faster migration lifecycles\.

**Note**  
*Packaging* refers specifically to the process of capturing a legacy application using the EMP Package Builder on the source operating system\.

**Always package on the source server**  
The [EMP Application Packaging Model](emp-packaging-model.md) demonstrates that the application packaging step occurs on the source server\. For example, if an application installs and functions on Windows Server 2008 R2, then the packaging step is performed on the same operating system\. Technically, it is possible to package the application on the operating system that the application is migrating to, such as Windows Server 2019\. However, packaging on the legacy OS version ensures that the application installs and functions as expected\. The application may fail to install and function on the modern operating system and, as a result, the install capture process on the modern operating system can result in a package created in a non\-working state\. 

**Always package on a clean server**  
After an application has been installed or packaged on a server, we recommend that you do not repackage the application on the same server in the same state\. This is because the EMP Package Builder compares the differences between snapshots taken before and after the application installation on the operating system\. If any legacy application components are on the operating system before the snapshot is taken, the resulting EMP package will not include the application components\. 

Creating snapshots of your packaging machine before carrying out any packaging would mean you could always restore your system to a clean state if you needed to recapture the application\. 

**Disable background noise**  
Turn off Microsoft Defender Antivirus, any other antivirus software, automatic Windows Update, and any Windows Security alerts to ensure that changes to the system by any of these background tasks are captured in the package\. If your security policy requires you that you keep antivirus software turned on, then you can unload them only when running Package Builder\. 

**Application prerequisite software or dependencies**  
If an application requires prerequisite software, for example, a legacy version of Java, and this software is not included or installed natively in the target operating system, then we recommend that you include these components in the EMP package for the application\. 

If an application requires operating system dependencies, for example, specific Windows roles and features, or requires an application prerequisite software that will be included on the target operating system, then we recommend that you set up these dependencies on the packaging server before the application is captured using the EMP Package Builder\. 

**Familiarize yourself with the application being packaged**  
We recommend that you document how the application installation is completed, and also the application workflows\. This helps to ensure that you can refer back to the documentation during the packaging phases\. This also helps to ensure that you can review the finalized list of steps required to capture a complete working state of the application\. The document should list known issues to watch out for and help you plan how dependencies must be captured or migrated\. We recommend that you perform a test install of the application before starting the EMP process\. 

**One package per server**  
When possible, we recommend that you create a single EMP package for each server that is being migrated\. This ensures a simple package design to help facilitate migration and future management of the application\. 

**Discovery**  
We recommend that you understand how to structure an EMP package before you create it in order to ensure successful package creation\. To understand the package structure and how the application works in its environment, we recommend that you perform application discovery\. You can use this information to structure the EMP package\. 
