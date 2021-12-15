# Uninstall an EMP package<a name="emp-uninstall"></a>

When you uninstall a package, all of the files, shortcuts, file type associations, and registry keys associated with the package are removed\. Files in use, which cannot be deleted, are marked for deletion for the next reboot\. After reboot, Windows removes the files marked for deletion along with the registry configuration for `Out of Process COM virtualization`\.

To uninstall a package, perform the following steps\.

1. Open a command prompt as an administrator\.

1. Run the following command, where `<source-location-of-package>` is the location from which the package was initially deployed\.

   ```
   C:\<source-location-of-package>\Compatibility.Package.Deployment.exe /U
   ```
**Note**  
If you run this command from the deployed location, the uninstall will be incomplete\. Verify that the package is uninstalled from the correct source path\.