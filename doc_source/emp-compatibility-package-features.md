# EMP compatibility package features<a name="emp-compatibility-package-features"></a>

The EMP compatibility package \(`compatiblity.package.engine.exe`\) includes features that can be enabled to resolve compatibility issues with packaged applications\. This topic defines these features and demonstrates how they work\.


**EMP compatibility package features**  

| Feature name | Description | 
| --- | --- | 
| ForceExternalManifest | Forces the use of an external manifest file, which overrides the system default\. | 
| RegClassesMerging | Merges virtual and real registry values into a virtual key\. | 
| DoNotHideDebugger | Ensures that process debugging remains visible\. | 
| HandleInvalidHandle | Ignores invalid handle errors\. | 
| NotWow64Process | Forces 32\-bit applications running on 64\-bit Windows to be virtualized as if they are running on 32\-bit Windows\. | 
| NetworkRedirection | Redirects hostnames, domain names, IPs, and ports\. | 
| LocalMappedObjectShim | Converts global file mappings to local\. | 
| DEPOptOut | Identifies and handles DEP exceptions\. | 
| ExcludeNativeWindows | Prevents a packaged application from using a native Windows application or process\. | 
| COMVirtualization | Virtualizes COM\. | 
| ForceWindowsVersion | Forces a Windows version check to a specific Windows version\. | 
| RedirectX64PackagedRegistry | Redirects 64\-bit registry when running a 32\-bit application\. | 
| LoadSystemResources | Loads architecture\-independent resource files regardless of bitness\. | 

**Topics**
+ [ForceExternalManifest](#emp-forceexternalmanifest)
+ [RegClassesMerging](#emp-regclassesmerging)
+ [DoNotHideDebugger](#emp-donothidedebugger)
+ [HandleInvalidHandle](#emp-handleinvalidhandle)
+ [NotWow64Process](#emp-notwow64process)
+ [NetworkRedirection](#emp-networkredirection)
+ [LocalMappedObjectShim](#localmappedobjectshim)
+ [DEPOptOut](#emp-depoptout)
+ [COMVirtualization](#emp-comvirtualization)
+ [ForceWindowsVersion](#emp-forcewindowversion)
+ [RedirectX64PackagedRegistry](#emp-redirectx64packagedregistry)
+ [LoadSystemResources](#emp-loadsystemresources)

## ForceExternalManifest<a name="emp-forceexternalmanifest"></a>

Windows includes a global setting in: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\PreferExternalManifest` \- DWORD that controls whether external manifests should be used when a process is launched \(or DLL loaded\)\. By default, this registry entry is missing, with a value of `0` \(disable\)\. This means that Windows uses an embedded manifest from the `exe/dll`, if one exists\.

The feature `ForceExternalManifest` allows the process launched by EMP to use an external manifest file\.

An application manifest is an XML file that describes and identifies the shared and private side\-by\-side assemblies to which an application should bind at runtime\. The name of an application manifest file is the name of the application's executable followed by `.manifest`\. 

The following is an example of an application manifest that disables Windows theming, which can cause compatibility issues with an earlier application version\.

```
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" xmlns:asmv3="urn:schemas-microsoft-com:asm.v3" >
 ...
  <asmv3:application>
    <asmv3:windowsSettings xmlns="http://schemas.microsoft.com/SMI/2011/WindowsSettings">
      <disableTheming>true</disableTheming>
    </asmv3:windowsSettings>
  </asmv3:application>
 ...
</assembly>
```

## RegClassesMerging<a name="emp-regclassesmerging"></a>

If an application uses an enumerate key function to determine available keys, the compatibility package redirections may not account for this\. Adding a high\-level redirection to resolve this behavior can result in more issues and prevent your application from functioning\. For example, if the application is enumerating `HKLM\Software\Classes` to determine available classes, adding a redirection for `HKLM\Software\Classes` will most likely result in a failure\. The solution is to enable registry merging so that the redirected and local registry keys are seen as one registry\. 

The following compatibility engine configuration example enables the `RegClassesMerging` feature in the `Compatibility.Package.Engine.clc` file\. 

```
<AAV>
    <Features>
        <Feature>RegClassesMerging</Feature>
    </Features>
</AAV>
```

The following example adds a `KeyMatch` redirection rule for the registry key in the previous example to `Redirections.xml` \.

```
<KeyMatch>
    <From>HKLM\SOFTWARE\Classes</From>
</KeyMatch>
```

## DoNotHideDebugger<a name="emp-donothidedebugger"></a>

This feature ensures that process debugging remains visible\. If not enabled, some applications will pass exceptions or breakpoints to the debugger \(engine\) and some applications can break\. This feature is usually enabled to prevent tampering or for license protection, and is often not required\.

The following compatibility engine configuration example enables the `DoNotHandleDebugger` feature in the `Compatibility.Package.Engine.clc` file\. 

```
<AAV>
    <Features>
        <Feature>DoNotHideDebugger</Feature>
    </Features>
</AAV>
```

## HandleInvalidHandle<a name="emp-handleinvalidhandle"></a>

When invalid handles cause applications to fail to run when using `DoNotHideDebugger`, then the compatibility engine can be configured to ignore the invalid handles\. If a thread uses a handle to a kernel object that is invalid \(for example, because it is closed\) Windows notifies the debugger\. The EMP engine handles any invalid handles by default\. However if `DoNotHideDebugger` is enabled, the EMP engine does not handle invalid handles\. If the `HandleInvalidHandle` feature is set, then the exception is handled and invalid handles are ignored\. 

 The following compatibility engine configuration example enables the `HandleInvalidHandle` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>DoNotHideDebugger</Feature>
        <Feature>HandleInvalidHandle</Feature>
    </Features>
</AAV>
```

## NotWow64Process<a name="emp-notwow64process"></a>

When enabled, this feature prevents Windows from using WOW64 redirections and forces processes to run as 32\-bit in the package by hooking the `IsWow64`, `PrintDlgExA`, `PrintDlgExW`, `PrintDlgA`, and `PrintDlgW` APIs\. 

 The following compatibility engine configuration example enables the `NotWow64Process` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>NotWow64Process</Feature>
    </Features>
</AAV>
```

**Note**  
This feature, when enabled, causes the printer driver host for 32\-bit applications \(`SPLWOW64.exe`\) to fail, and must be excluded if the packaged application launches it\.

## NetworkRedirection<a name="emp-networkredirection"></a>

The `NetworkRedirection` feature enables network redirection for hostname, domain name, IP, and ports\. This allows server applications running in packages to redirect their network requests to new names, IP addresses or ports, so that you can migrate applications to new servers without changing application source code\.

**Use cases**
+ Hostname virtualization with `<Network ThisComputer="legacy_machine_name">` applies to server applications for which you don't have the installation media, and the application has been extracted from the server on which it runs\. This feature virtualizes the hostname of the server on which the application runs so that it behaves as if it runs on the original server\.
+ Domain Name redirection with `<DomainName>` is for server applications for which you don't have the installation media, and the application has been extracted from the server on which it runs\. This feature redirects from the name a server application expects to find on the network to one that is present\. 
+ IP address and port redirection with `<Connect>` enables applications to accept connections `<From>` one IP address or port `<To>` another\. 

**Note**  
Apply the Microsoft server naming conventions described at [https://docs\.microsoft\.com/en\-us/troubleshoot/windows\-server/identity/naming\-conventions\-for\-computer\-domain\-site\-ou](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/naming-conventions-for-computer-domain-site-ou)\. These are the only naming conventions supported\. 

The following compatibility engine configuration examples enable the `NetworkRedirection` feature in the `Compatibility.Package.Engine.clc` file\.

**Hostname virtualization**

```
<AAV>
   <Features>
      <Feature>NetworkRedirection</Feature>
  </Features>
  <Network ThisComputer="legacy_machine_name">
  </Network>
</AAV>
```

**Domain name redirection**

```
<AAV>
   <Features>
        <Feature>NetworkRedirection</Feature>
   </Features>
   <Network>
     <DomainName>
        <From>legacy_machine</From>
        <To>new_machine</To>
    </DomainName>
  </Network>
</AAV>
```

**IP and port redirection**

 The following example redirects network connections from `192.168.2.1` on port `13000` to `127.0.0.1` on port `12000.` 

```
<AAV>
    <Features>
        <Feature>NetworkRedirection</Feature>
    </Features>
    <Connect>
        <From>
            <IP>192.168.2.1</IP>
            <Port>13000</Port>
        </From>
        <To>
            <IP>127.0.0.1</IP>
            <Port>12000</Port>
        </To>
    </Connect>
</AAV>
```

**Logging**  
When the `<DomainName>` feature is enabled, the compatibility engine logs the following\.

```
Server redirection from %old% to %new%
```

Only the Microsoft server naming conventions described at [https://docs\.microsoft\.com/en\-us/troubleshoot/windows\-server/identity/naming\-conventions\-for\-computer\-domain\-site\-ou](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/naming-conventions-for-computer-domain-site-ou) are supported\. The following messages are logged if a server name does not apply these naming conventions\.

```
Server name cannot begin with a period (.) and will not be virtualized
```

```
Server name cannot be longer than 15 characters and will not be virtualized
```

```
Server cannot contain invalid character "invalid_character" and will not be virtualized
```

If `<To>` is not specified, the following message is logged\.

```
Server redirection will not redirect from %old%
```

If `<From>` is not specified, the following message is logged\.

```
Server redirection will not redirect to %new%
```

## LocalMappedObjectShim<a name="localmappedobjectshim"></a>

The name of the file mapping object can include a `Global` or `Local` prefix in order to create the object in the global or session namespace\. If a service or system creates a file mapping object in the global namespace, any process running in any session can access that file mapping object if the caller has the required access rights\. To ensure that the kernel object names created by your applications do not conflict with the names of any other applications, enable the \*`LocalMappedObjectShim` feature\. This feature converts all file mapping objects from the global to local namespace if no redirection rule is set for the object name\. 

**Use cases**
+ Enable an application that requires administrator permissions to run on an account with lower permissions\.
+ Enable multiple instances of the desktop application to run on a server operating system when the use of global objects by the application prevents the application from installing\. 

The following compatibility engine configuration examples enable the `LocalMappedObjectShim` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>LocalMappedObjectShim</Feature>
    </Features>
</AAV>
```

 File mapping exclusions can be applied for named file mapping objects so that they remain global objects by including the following tags\.

```
<FileMappingExclusions>
    <FileMappingExclusion>Global\string</FileMappingExclusion>
</FileMappingExclusions>
```

## DEPOptOut<a name="emp-depoptout"></a>

Applications written using Visual Studio 2008, or earlier, are incompatible with operating systems enabled with Data Execution Prevention \(DEP\), this includes the following\.
+ Systems configured with Secure Boot\. 
+ Default policies applied to the Windows operating system\.
+ Windows running the Enhanced Mitigation Experience Threat \(EMET\) toolkit\. 

This incompatibility is caused by the forcing of DEP enablement for an application\. 

**Use cases**
+ Enable the application to opt out of DEP, so that it can run on the server or desktop without configuration changes for EMET, or the default policies that are applied to an application within its organization\.
+ For applications running in the compatibility package, the `DEPOptOut` feature resolves memory access violations by changing the memory address location to an executable part of memory\.

**Identify whether a failure is due to DEP**  
If a failure is caused by DEP, the application crashes with or without an error message\. When a detailed error message is displayed, it shows the exception details, which include: Exception Code: c0000005, which means `ACCESS VIOLATION`, and `Exception Data: 0000008`\. 

On later versions of Windows, the message doesn’t display the exception details\. You must look at the Windows application event log\. `Error Event 1000` will report the exception code `C0000005`\.

The following compatibility engine configuration example enables the `DEPOptOut` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>DEPOptOut</Feature>
    </Features>
</AAV>
```

## COMVirtualization<a name="emp-comvirtualization"></a>

The `COMVirtualization` feature is required to virtualize the out\-of\-process COM servers\. If the `ComDeployment.xml` file in the compatibility package contains one or more `OutOfProcServer` entries, then you must enable the `COMVirtualization` feature\.

The following compatibility engine configuration example enables the `COMVirtualization` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>COMVirtualization</Feature>
    </Features>
</AAV>
```

## ForceWindowsVersion<a name="emp-forcewindowversion"></a>

If an application requires a particular version of the operating system in order to run, it queries the operating system to ensure that the expected version, build, service pack, or type \(desktop or server\) is returned\. EMP compatibility packages can intercept the API requests and return values specified by the compatibility engine configuration file\. 

The following compatibility engine configuration example enables the `ForceWindowsVersion` feature in the `Compatibility.Package.Engine.clc` file\.

Possible values for `ProductType` are:
+ `Server`
+ `DomainController`

```
<Features>
   <Feature>ForceWindowsVersion</Feature>
</Features>
<ForceWindowsVersion>
   <MajorVersion>INT</MajorVersion>
   <MinorVersion>INT</MinorVersion>
   <BuildNumber>INT</BuildNumber>
   <ProductType>TYPE</ProductType>
   <ServicePackText>INT</ServicePackText>
   <ServicePackMajor>INT</ServicePackMajor>
   <ServicePackMinor>INT</ServicePackMinor>
</ForceWindowsVersion>
```

**Example scenario**  
An EMP\-packaged application must run on Windows Server 2019\. However, it fails with an error message stating that the application must be installed on Windows Server 2003\. This is because it checks whether the operating system is Windows Server 2003 and finds Windows Server 2019\. If the application does not require a specific service pack, `ForceWindowsVersion` can be configured as follows\.

```
<Features>
   <Feature>ForceWindowsVersion</Feature>
</Features>
<ForceWindowsVersion>
   <MajorVersion>5</MajorVersion>
   <MinorVersion>2</MinorVersion>
   <BuildNumber>3790</BuildNumber>
</ForceWindowsVersion>
```

## RedirectX64PackagedRegistry<a name="emp-redirectx64packagedregistry"></a>

The Compatibility Package Builder detects whether it is running on a 64\-bit operating system and writes the `<Feature>RedirectX64PackagedRegistry</Feature>` configuration to the `clc` file so that the package knows which platform that it was created on\. Packages created on a 32\-bit operating system do not require this compatibility feature\. 

The following compatibility engine configuration example enables the `RedirectX64PackagedRegistry` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>RedirectX64PackagedRegistry</Feature>
    </Features>
</AAV>
```

## LoadSystemResources<a name="emp-loadsystemresources"></a>

This feature loads architecture\-independent resource files regardless of bitness\. The `LoadSystemResources` feature is useful when 32\-bit applications must access the resource\-only DLL present in the native system32 directory instead of syswow64\. This is necessary when the wow64 file system redirection is enabled for a 32\-bit application\. 

The following compatibility engine configuration example enables the `LoadSystemResources` feature in the `Compatibility.Package.Engine.clc` file\.

```
<AAV>
    <Features>
        <Feature>LoadSystemResources</Feature>
    </Features>
</AAV>
```