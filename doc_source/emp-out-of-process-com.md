# Enable support for out\-of\-process Common Object Model \(COM\) in an EMP package<a name="emp-out-of-process-com"></a>

This topic includes steps to enable support for out\-of\-process Common Model Objects \(COM\) in an EMP package\. There are two main types of COM servers: in\-process and out\-of\-process\. In\-process servers are implemented in a dynamic linked library \(DLL\)\. Out\-of\-process servers are implemented in an executable file \(EXE\)\. 

Out\-of\-process servers can reside on either the local computer or a remote computer\. In addition, COM provides a mechanism that allows an in\-process server \(DLL\) to run in a surrogate EXE process to run the process on a remote computer\. For more information about how to create DLL servers that can be loaded into a surrogate EXE process, see the Microsoft documentation at [DLL Surrogates](https://docs.microsoft.com/en-us/windows/win32/com/dll-surrogates)\.

Applications with components that run out\-of\-process require the `COMVirtualization` feature to be enabled to virtualize COM\. Process Monitor \(procmon\) can be used to detect these components by monitoring `SVCHOST.exe`\. Also, `APIMON` can be used, filtering for `CoCreateInstance`\.

A failure results in either `CLSID not found` or `Component not registered` when the COM subsystem does not run within the virtual environment of the package\.

**Note**  
The `COMVirtualization` feature is not required if the application uses in\-process COM objects\. Applications that use in\-process COM objects behave as expected without enabling the feature\.

**Enable the `COMVirtualization` feature**

1. Navigate to the package folder of the application\.

1. Edit the `Compatibility.Package.Engine.clc` in a text editor, such as Notepad\+\+\.

1. Enable the `COMVirtualization` feature by moving the feature tag out of the section of tags that are commented out\.

   ```
   <Features>
       <Feature>COMVirtualization</Feature>
   </Features>
   ```

   For a 64\-bit package, add the `COMVirtualization` tag to the automatically added `<Features>` element that contains the `RedirectX64PackageRegistry` feature\. If two `<Features>` elements are used in the CLC file, an error will result when you attempt to run the package\.

   ```
   <AAV>
     <Features>
       <Feature>RedirectX64PackagedRegistry</Feature>
       <Feature>COMVirtualization</Feature>
     </Features>
   </AAV>
   ```

**Debugging COM virtualization**
+ COM messages are written to the engine logs\. Search for `COM` to find them\.
+ COM error messages are written to the engine logs\. Search for `ERROR, COM` to find them\.
+ If COM instance creation fails, the engine log message will contain `Failed to create COM instance`\.
+ Resolving or troubleshooting problems with COM Virtualization may require temporary or permanent exclusion of CLSIDs from the process\.

**To exclude CLSIDs from COM virtualization**

  1. In the `Compatibility.Package.Engine.clc` configuration file, locate and edit the `<Features>` tag, which is used to enable COMVirtualization\.

     ```
     <AAV>
        <Features>
          <Feature>ComVirtualization</Feature>
        </Features>
     </AAV>
     ```

  1. To exclude CLSID `EF66E233-9F07-4E32-9119-FF40CDDD4DCF`, insert the following `<COM>` tag code outside of the `<Features>` tag to specify the CLSID you want to exclude\.

     ```
     <AAV>
       <Features>
         <Feature>ComVirtualization</Feature>
       </Features>
       <COM>
         <CLSID ID="{EF66E233-9F07-4E32-9119-FF40CDDD4DCF}" Excluded="true">
          </CLSID>
        </COM>
     </AAV>
     ```

     To include the CLSID, change the value of `Excluded` to `="false"`, or remove the `<COM>` tags\.