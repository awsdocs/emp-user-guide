# Add side\-by\-side \(SXS\) assemblies to an EMP compatibility package<a name="emp-sxs-assemblies"></a>

A Windows side\-by\-side assembly is described by [manifests](https://docs.microsoft.com/en-us/windows/win32/sbscs/manifests)\. A side\-by\-side assembly contains a collection of resources — a group of DLLs, Windows classes, COM servers, type libraries, or interfaces — that are always provided to applications together\. These resources are described in the assembly manifest\. 

Typically, a side\-by\-side assembly is a single DLL\. For example, the Microsoft COMCTL32 assembly is a single DLL with a manifest, whereas the Microsoft Visual C\+\+ development system run\-time libraries assembly contains multiple files\.

Manifests contain [metadata](https://docs.microsoft.com/en-us/windows/win32/sbscs/m-sbscs-gly) that describes side\-by\-side assemblies and side\-by\-side assembly dependencies\. Side\-by\-side assemblies are used by the operating system as fundamental units of naming, binding, versioning, deployment, and configuration\. 

Every side\-by\-side assembly has a unique identity\. One of the attributes of the assembly identity is its version\. For more information, see [Assembly Versions](https://docs.microsoft.com/en-us/windows/win32/sbscs/assembly-versions) in the Microsoft documentation\.

The EMP Compatibility Package Builder does not configure support for side\-by\-side assemblies\. If your application uses side\-by\-side assemblies that are distributed with installers, you must first attempt to install them natively on the target operating system\. If this is successful, you can script the installation of these assemblies into package deployment using the `DeploymentScript.xml`\. For more information see [Managing EMP custom configurations](emp-custom-configurations.md)\. If native installation is not possible or does not address the issue, you can add the assemblies to the package as private assemblies\.

The following error message will appear in the Windows Event Viewer, from which you can determine what assemblies you need, along with the version: 

```
Activation context generation failed for "DemoApplication.exe". Dependent AssemblyTowersWatson.Components.Licensing.ComSRM,publicKeyToken="97c62a3c455f5e0d",type="win32",version="2.0.5.48761"could not be found. Please use sxstrace.exe for detailed diagnosis.
```

**Add side\-by\-side assemblies to an EMP package**

1. Add the missing DLLs associated with the run\-times to the same folder as the executable being run\. Alternatively, use Process Monitor to monitor the application process along with `csrss.exe` to determine where the application is looking for these DLLs\. 

1. Locate the DLLs \(usually located in `C:\Windows\Assembly` or `C:\Windows\WinSXS` folders\)\. 