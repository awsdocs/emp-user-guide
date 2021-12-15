# Run `cmd.exe` as a child process to the EMP compatibility package engine<a name="emp-run-cmd-child"></a>

This topic explains how to test or troubleshoot an EMP package by launching `cmd.exe` as a child process to the EMP package engine\. Doing this launches `cmd.exe` in the context of the EMP package to quickly run tests, validate application errors, and retest functionality\.

**Run `cmd.exe` as a child process**

1. Deploy the EMP package following the usual deployment process\.

1. Open a new `cmd.exe` window and navigate to the root of the EMP package\. Run the following command\.

   ```
   <pathtopackage>\Compatibility.Package.Engine.exe /F cmd.exe
   ```

   This command launches the engine and runs the `cmd.exe` as a child process in the context of an EMP package\. A new command window will open\. All redirection rules and package configurations now apply to the new `cmd.exe` process\. You can verify this using Process Explorer and looking for the `cmd.exe` under the parent `cmd.exe`\.

**Example: Verify that application can access its main installation folder**

In this troubleshooting scenario, we want to verify that the `LegacyApp` application packaged in EMP can access its main installation folder, `LegacyAppFolder`\.

1. Check the package to confirm that `LegacyAppFodler` exists\.  
![\[LegacyAppFolder\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-legacy-app-folder.png)

1. A corresponding `<FolderMatch>` redirection should exist in the `Redirections.xml` file of the package\.

   ```
   <FileSystem>
       <FolderMatch>
           <From>%ProgramFilesX86%\LegacyAppFolder</From>
           <To>ProgData\LegacyAppFolder</To>
       </FolderMatch>
   ```

1. Use `cmd.exe` to check for the file in the local system\. The system should not be able to find the file\.  
![\[Search for LegacyAppFolder in local system\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-legacy-app-folder-not-found.png)

1. Run `cmd.exe` as a child process to the package engine\. This should confirm that the `LegacyAppFolder` is available in the context of the EMP package\.  
![\[Search for LegacyAppFolder in context of package\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-legacy-app-folder-child-process.png)