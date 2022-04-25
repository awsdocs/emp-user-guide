# How AWS End\-of\-Support Migration Program \(EMP\) for Windows Server works<a name="emp-how-it-works"></a>

AWS End\-of\-Support Migration Program \(EMP\) for Windows Server technology identifies the dependencies and resources for your application, and creates an application package from them\. This application can be deployed to and run on a new version of Windows Server\.

The EMP toolset creates packages for legacy Windows applications to run on any supported version of the operating system\. It includes a set of packaging tools, which are used to address various packaging scenarios\. When a package is created, application runtime analysis detects application file and registry activity, including additional configuration changes that occur when the application first runs\. The EMP package is deployed with a command passed to its deployment program\. Deployment can be automated with scripts or managed by enterprise tools, such as AWS Systems Manager\.

Application compatibility packages include everything an application requires to run on a modern operating system\. This includes all of the application files, runtimes, components, deployment tools, and the embedded redirection and compatibility engine\. During the creation of a package, EMP determines the application dependencies for the out\-of\-support operating system, and sets the required redirections\. The redirection and compatibility engine are designed to run in user mode\. When the package is deployed to the target machine, the file type associations are registered, and shortcuts are created to run the application\.

Compatibility packaging follows these three principles: 
+ Packaging is performed on the operating system supported by the application\.
+ Runtime analysis is performed to detect and review the file and registry activity of the processes of the application\. For a fresh install capture, runtime analysis detects first\-run activity of the application after installation\.
+ The redirection and compatibility engine, along with the package deployment program, are included in the package\.

The package does not include the legacy operating system\. This means that you do not run any part of the legacy Windows Server version on the new Windows Server to which the application is upgraded\. The redirection engine intercepts the API calls that the application makes to the underlying Windows Server operating system, and requests for files and registry data captured in the package are redirected there\. As a result, the request by the application for resources that are present within the package are redirected to the package so that the application runs successfully on the new operating system\. This lightweight strategy means that the target application can see a combination of the redirected and local file system and registry, with undetectable impact on the performance of the application or local machine\. The approximate RAM overhead per application is 3 to 5 MB, with no measurable CPU impact after the application has started\. 

Common redirections, which do not include file and registry redirections, supported by the EMP compatibility engine include: 
+ **The application uses a fixed port**\. The EMP engine redirects to an appropriate and available port on the new version of the operating system\. 
+ **The application accesses data stored in a fixed location that is not available on the new OS version**\. The EMP engine redirects these requests to the appropriate location on the new version of the operating system\. 
+ **The application is hardcoded to `C:\WinNT`**\. The EMP engine redirects the application to `C:\Windows`\. 
+ **The application requires Java version 1\.3**\. EMP packages and isolates the application with Java 1\.3 so that it does not make calls to newer Java runtimes in the new version of the operating system\. 

**Topics**
+ [Phases of the EMP process](emp-steps.md)
+ [System requirements](emp-supported-os.md)
+ [Limitations](emp-limitations.md)
+ [Data collection](emp-data.md)
+ [Compatibility Package Builder components](emp-components.md)