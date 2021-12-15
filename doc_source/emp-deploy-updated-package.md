# Update a deployed EMP package<a name="emp-deploy-updated-package"></a>

This topic contains information to guide you through the process of updating an already deployed EMP package\.

To update the contents of an already deployed EMP package with a new version of the package, run the `Compatibility.Package.Deployment.exe` command with the `/update` switch\.

```
Compatibility.Package.Deployment.exe /update 
```

The package may include changes to the following:
+ Files
+ Registry settings
+ Shortcuts
+ Package configuration files
+ File type associations

**Important**  
If you attempt to use the `/deploydir` switch when a package has already been deployed, a `Failed to deploy' exit code -1` error will be returned\. The `/update` switch must be used to update the package to the latest version, or the `/uninstall` switch must be used to remove the package first\.

Each component is updated as follows\.

**File Associations**  
If the file type associations source file \(`FileAssociations.xml`\) in the new package is the same as the one in the currently deployed package, `/update` will recreate any missing file type associations and restore values or types to the original values and types specified during the initial deployment\. 

**Note**  
The `/update` switch preserves any values that appear in the registry that are not specified in the source file\. If the file type associations source file \(`FileAssociations.xml`\) in the new package is different from the one in the currently deployed package, `/update` deletes the registry values that do not appear in `FileAssociations.xml` and updates values and types that have changed\.

**Shortcuts**  
New shortcuts specified in `Shortcuts.xml` will be created\. Shortcuts that do not exist in the XML file will be removed\. Fields that are different between the currently deployed and the new version to be deployed will be updated to the latest version\.

**Registry**  
A registry update will be performed the next time the application runs\. This does not happen as part of the update process\. Updates are performed if the value of `Last Modified Date of Registry Added` in `HKCU\Software\AWSEMP\Compatibility.Package\{appid}` differs from value of the last modified date of the new application registry XML file \(`AppRegistry.xml`\)\.

The registry update removes all keys under `{appid}`, but not the values of the keys, and creates all of the entries specified in the `AppRegistry.xml` file\. The update then sets the last modified time to the time of the new `AppRegistry.xml file`\. 

Subsequent application start events will not initiate registry updates because the modified time of the file will match the value stored in the registry\. If the last modified time cannot be found in the registry, the registry will be created using the latest `AppRegistry.xml file` file\. If the `AppRegistry.xml file` file is invalid, `/update` will report an error and will not remove any application registry\. 