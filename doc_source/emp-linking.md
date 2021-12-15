# Link EMP packages<a name="emp-linking"></a>

This topic contains an example `Redirections.xml` file to show you how to link EMP packages so that they can interact with each other\. One scenario for which this process is useful is if an application requires access to files or registry keys from a different EMP package, for example, if you packaged a dependency in a separate EMP package and the main application requires access to the dependency application to function as required\.

You can link packages by loading the `Redirections.xml` file from one package to another using the `<Includes>` tag in the EMP compatibility engine config file \(`Compatibility.Package.Engine.clc`\)\.

**Example**  
In the following example, an `<Include>` tag entry has been added \(`C:\DeployDirForPackage2\ExamplePackage2\Redirections.xml`\)\. When a process from `ExamplePackage1` starts, the engine that starts the process will load the `Redirections.xml` file from both `ExamplePackage1` and `ExamplePackage2`\.

```
<AAV PackageId="ExamplePackage1">
<Includes>
    <Include>Redirections.xml</Include>
    <Include>C:\DeployDirForPackage2\ExamplePackage2\Redirections.xml</Include>
</Includes>
```

When both `Redirections.xml` files are loaded, the process from `ExamplePackage1` follows all of the redirections from `ExamplePackage2`\. In this example, we could add an `<Include>` entry for the `Redirections.xml` for `ExamplePackage1` in the `Compatibility.Package.Engine.clc` of `ExamplePackage2` so that processes from either package can access each other\.

In this example, a relative path is used\. If `ExamplePackage1` and `ExamplePackage2` are deployed into the same `DeployDir`, you can enter the path as follows: `<Include>..\ExamplePackage2\Redirections.xml</Include>`\.