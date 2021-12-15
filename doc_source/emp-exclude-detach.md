# Exclude or detach a process from the package redirection rules<a name="emp-exclude-detach"></a>

This topic explains how to use the `Excludes` or `Detaches` features in the package redirection rules\.

**`Excludes` feature**  
We recommend the `Excludes` feature method to control the behavior of the package redirection and virtualization engine\. It prevents child processes from being virtualized and redirected without affecting the virtualization of the parent processes\.

This feature is useful when the packaged application is spawning local processes from locally installed applications that you do not want redirected\. This is because the redirections change the behavior of the child processes\.

**Exclude a process from the package redirection rules**

1. Open `Compatibility.Package.Engine.clc`\.

1. Add the executable names to the `Exclude` tags\.

   ```
   <Excludes>
   <Exclude>Excel.exe</Exclude>
   <Exclude>Winword.exe</Exclude>
   </Excludes>
   ```

**`Detaches` feature**  
You can control the behavior of the virtualization and redirection engine using the `Detaches` feature\. After an application has started, the virtualization engine exits when the initial create process function completes\. All redirections and most virtualization features will be available to the parent process that started\. 

The `Detaches` feature is useful when child processes do not exit cleanly, and terminate with an exception\.

**Important**  
The virtualization and redirection engine of the package will detach from the parent process once virtualization is complete\. Any child processes will not be virtualized, and the detached process will not benefit from `DEPOptOut` or `HandleInvalidHandle`, even if these features are enabled\.

**Detach a process from the package redirection rules**

1. Open `Compatiblity.Package.Engine.clc`\.

1. Add the executable names to the `Detach` tags\.

   ```
   <Detaches>
   <Detach>Excel.exe</Detach>
   <Detach>Winword.exe</Detach>
   </Detaches>
   ```