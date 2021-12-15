# EMP compatibility package contents<a name="emp-package-contents"></a>

This section describes the folders and files that are included in an EMP compatibility package\. When the EMP compatibility packaging process is complete, the output of the package builder is called an EMP compatibility package\. The package contains both the file and registry data of the packaged application, and the EMP binaries and configuration files that are required to deploy and run the packaged application\. 

The EMP package, which is the product to deploy, is called the source package\. The post\-deployment package is called the deployed package\. These packages are slightly different at the level of the root package folder\. However, the packaged application content is the same in both packages\.

**Topics**
+ [Source package contents](#emp-package-contents-source)
+ [Deployed package contents](#emp-package-contents-deployed)

## Source package contents<a name="emp-package-contents-source"></a>

The source package root folder contains two folders, along with a list of files\.
+ **ProgData folder** — Contains the directory structure and files of the packaged application, captured during the EMP packaging process\. 
+ **EngineBinaries folder** — Contains the EMP package engine binaries, which support both 32\-bit and 64\-bit Windows operating systems\. During deployment, package deployment detects the bit rate of the operating system and deploys the appropriate engine binaries into the deployed package\. Both 32\-bit and 64\-bit binaries are deployed into 64\-bit Windows operating systems\.

**Files**
  + **Compatibility\.Package\.Engine\.exe** — The compatibility package redirection \(virtualization\) engine\. When the entry point of an application is invoked, the engine is launched with the arguments required to load the target application process as a child process of the engine\. As a result, the engine is able to intercept and redirect calls from the process\.
  + **Compatibility\.Package\.Engine\.Launcher\.x64\.exe** — Along with its 32\-bit counterpart, this executable file an out\-of\-process COM server launcher\. For packages running on 32\-bit Windows operating systems, only the x86 program is required\. However, both programs are required on 64\-bit Windows operating systems\. 
  + **Compatibility\.Package\.Engine\.x64\.dll** — Along with its 32\-bit counterpart, this \.dll is a library that is used by the compatibility package engine\. Both libraries are required for a package to run on a 64\-bit Windows operating system\. However, only the 32\-bit library is required for 32\-bit Windows operating systems\.
  + **HookYou\.exe** and **HookMe\.dll** — These files provide an alternative method to virtualize a process when it has not been started by the package engine\.
+ **Other source package files**
  + **\_metadata\.json** — Contains EMP package metadata\.

    ```
    {
    "PackageId": "SQL2005EXP_9482",
    "Icon": "%DefaultDir%\\Compatibility.Package.Run.exe",
    "Name": "SQL2005EXP",
    "Version": "",
    "Publisher": "",
    "AWSProfileName": "default"
    }
    ```
  + **Compatibility\.Package\.Deployment\.exe** — The package deployment program, called with arguments to deploy, update, or uninstall an EMP package\. By default, it logs deployment events to a text file located at the root of the package folder\. This program eliminates the need to install an agent on the target operating system\.
  + **Compatibility\.Package\.Deployment\.exe\.config** — Used to pass settings to the executable file, for example, the logging level\. The default level is `INFO`\. 
  + **DeploymentWorkFlowLog\.txt** — The log file created by the package deployment program\. Contains a log of deployment events according to the log level in `Compatiblity.Package.Deployment.exe.config`\. 
  + **AppRegistry\.xml** — The main registry file for the package\. Contains most of the registry data of the packaged applications\.

    ```
    <RegistryOperations xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" NeedToBeDecoded="true" ValidateWrite="false">
      <Write>
        <KeyName>HKEY_CURRENT_USER\Software\AWSEMP\Compatibility.Package\%GUID%\HKLM\SYSTEM\CurrentControlSet\services\eventlog\Application\Visual Studio - VsTemplate</KeyName>
        <ValueName>EventMessageFile</ValueName>
        <Value ValueType="ExpandString">%ProgramFilesX86%\Microsoft Visual Studio 8\Common7\IDE\msenv.dll</Value>
      </Write>
      <Write>
        <KeyName>HKEY_CURRENT_USER\Software\AWSEMP\Compatibility.Package\%GUID%\HKLM\SYSTEM\CurrentControlSet\services\eventlog\Application\Visual Studio - VsTemplate</KeyName>
        <ValueName>TypesSupported</ValueName>
        <Value ValueType="DWord">7</Value>
      </Write>
    ```
  + **ComDeployment\.xml** — Contains instructions for deploying out\-of\-process COM servers, COM\+, and DCOM components\. The setting `Enabled="true"` denotes that the deployment of out\-of\-process COM servers, as well as COM\+ and DCOM components, occurs during package deployment so that they are immediately available\. You may want to set `Enabled="false"` to prevent this kind of deployment for some troubleshooting scenarios\.

    ```
    <COM xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Enabled="true">
      <OutOfProcServers>
        <OutOfProcServer>
          <Path>%DefaultDir%\ProgData\Program Files (x86)\Microsoft Visual Studio 8\Common7\IDE\devenv.exe</Path>
        </OutOfProcServer>
      </OutOfProcServers>
    </COM>
    ```
  + **Compatibility\.Package\.Run\.exe** — The program that serves as the entry point into the EMP package\. When invoked, it verifies that the registry is loaded and consistent with the registry files contents\. If not, it loads the package registry before launching the engine with arguments to launch the target application program\. By default, it logs high\-level run events to a text file located at the root of the package folder\. 
  + **Compatibility\.Package\.Run\.exe\.config** — Used to pass settings to the package deployment executable, for example, the logging level\.
  + **Compatibility\.Package\.Engine\.clc** — The package engine configuration file\. Used to apply settings, such as compatibility features, links to other EMP packages, engine detach, and process exclusion from redirection\.

    The following is a sample of the default file that is included in each package\.

    ```
    <AAV PackageId="SQL2005_7206">
    <Includes>
        <Include>Redirections.xml</Include>
      </Includes>
      <!--
      <Excludes>
        <Exclude>SomeExecutable.exe</Exclude>
      </Excludes>-->
      <!--
      <Detaches>
        <Detach>SomeExecutable.exe</Detach>
      </Detaches>-->
      <!--
      <COM>
        <CLSID ID="" Excludes="false">
        <Registration ConnectionType = "MultiUse"/>
        </CLSID>
      </COM>-->
      <!--
      <Features>
        <Feature>DEPOptOut</Feature>
        <Feature>HandleInvalidHandle</Feature>
        <Feature>NetworkRedirection</Feature>
        <Feature>LocalMappedObjectShim</Feature>
        <Feature>NotWow64Process</Feature>
        <Feature>ForceWindowsVersion</Feature>
        <Feature>COMVirtualization</Feature>
      </Features>
      -->
      <Features>
        <Feature>RedirectX64PackagedRegistry</Feature>
      </Features>
    </AAV>
    ```

**Contents**
    + **Includes** — Element that specifies the `Redirections.xml` of the current package\. No path is required because it exists in the root folder of the packages\. Other packages can be linked to the current package by adding their `Redirections.xml` files here\. For more information, see [Link EMP packages](emp-linking.md)\.
    + **Excludes** and **Detaches** — For more information, see [Exclude or detach a process from the package redirection rules](emp-exclude-detach.md)\.
    + **COM** — Element that specifies class IDs \(CLSIDs\) that must be excluded from `COMVirtualization`\. For more information, see the steps for excluding CLSIDs from `COMVirtualization` under [Enable support for out\-of\-process Common Object Model \(COM\) in an EMP package](emp-out-of-process-com.md)\. 
    + **Features** — See [EMP compatibility package features](emp-compatibility-package-features.md)\.
  + **ComRegistryKeys\.xml** — Contains the COM, DCOM, and COM\+ registration data of the packaged application\. 
  + **DeploymentScript\.xml** — Introduces custom configurations into an EMP package\. See [Managing EMP custom configurations](emp-custom-configurations.md)\. 
  + **EMP\.TelemetryClient\.exe** — Program that collects some basic operational information about the usage of EMP to improve the product\. The ability of EMP to send telemetry data to AWS is mandatory to deploy packages\. For more information, see [Deploy an EMP package](emp-deploy.md)\. For data collected, see [Data collected by the AWS End\-of\-Support Migration Program \(EMP\) for Windows Server ](emp-security-data.md)\.
  + **EMP\.TelemetryClient\.exe\.config** — Passes settings to the Telemetry Client, for example, the `AWSProfileName`\.
  + **EnvironmentVariables\.xml ** — Contains the captured environment variables that are required to be available to the virtualized application\.

    ```
    <Variables xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Variable Name="Path" Append="true" IsEncoded="false" Value="C:\Program Files (x86)\Microsoft SQL Server\90\Tools\binn\" />
    </Variables>
    ```
  + **FileAssociations\.xml** — Contains the file associations registration data for the packaged application\.

    ```
    <RegistryOperations xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" NeedToBeDecoded="true" ValidateWrite="false">  <Write>
      <KeyName>HKEY_LOCAL_MACHINE\SOFTWARE\Classes\.xdl</KeyName>
        <ValueName></ValueName>
        <Value ValueType="String">ssmsee.xdl.9.0_SQL2005_7206</Value>
      </Write>
      <Write>
        <KeyName>HKEY_LOCAL_MACHINE\SOFTWARE\Classes\ssmsee.xdl.9.0_SQL2005_7206</KeyName>
        <ValueName></ValueName>
        <Value ValueType="String">Microsoft SQL Server Deadlock File</Value>
      </Write>
    ```
  + **Programs\.xml** — Specifies how EMP launches the programs of the packaged application\. Each set of instructions for running a program is called a `RunCondition`\. The `RunCondition` is an argument that is passed to `Compatibility.Package.Run.exe`\. Each run condition \(`run1`, `run2`, and so on\) is a launch instruction to package run, which contains an argument to pass to the package engine\. The package engine specifies what application program to launch, in what directory, for how long, and includes any arguments to be passed to it\.

    ```
    <Programs xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Program>
        <RunCondition>run1</RunCondition>
        <ProcessWindowStyle>Normal</ProcessWindowStyle>
        <Path>%DefaultDir%\Compatibility.Package.Engine.exe</Path>
        <Args>/f "%DefaultDir%\ProgData\Program Files (x86)\Microsoft SQL Server\90\Tools\Binn\VSShell\Common7\IDE\ssmsee.exe" %FILEARGS%</Args>
        <WorkingDirectory />
        <WaitCondition TimeoutInSeconds="0">None</WaitCondition>
      </Program>
    ```
  + **Redirections\.xml** — The redirection instruction set to the package engine, which specifies which file, folder, or registry key requests should be redirected and to where\. It also contains a duplicate of some of the content of the CLC file, which allows it to be configured with compatibility package features, COM exclusions, process exclusions or detaches, as required, when it is used to link the package to another compatibility package\.

    The following are examples of file, folder, and registry redirection rules\.

    ```
    <ExactMatch>
      <From>%SystemX86%\SQLServerManager.msc</From>
      <To>ProgData\Windows\SysWOW64\SQLServerManager.msc</To>
    </ExactMatch>
    ```

    ```
    <FolderMatch>
      <From>%ProgramFiles%\Microsoft SQL Server</From>
      <To>ProgData\Program Files\Microsoft SQL Server</To>
    </FolderMatch>
    ```

    ```
    <KeyMatch>
      <From>HKLM\SYSTEM\CurrentControlSet\services\SQLWriter</From>
    </KeyMatch>
    ```
  + **Report\.json** — Contains a report on any unsupported application features detected during the package build, for the attention of the packaging engineer\. 

    The following is a sample report for an unsupported COM\+ property\.

    ```
    [{
    "Level": "Warn",
    "Category": "COM+",
    "Message": "COM+ application DotNetTestMultiCOMPlusApp is using roles for managing the the security. At the moment roles are not ported across in the compatibility package. Please recreate all the roles on target machine manually after deploying the package. Please ensure you configure the roles at component, interface and method level similar to how they are configured at source machine."
    }]
    ```
  + **Services\.xml** — Contains instructions to configure any Windows services captured during packaging\. `ImagePath` specifies the package run program with a run condition\. The run condition is detailed in the `Program.xml` file\. So, although services are installed natively, the service image is virtualized\.

    ```
    <Services xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Service>
        <Name>MSSQL$SQLEXPRESS</Name>
        <ImagePath>%DefaultDir%\Compatibility.Package.Run.exe /RunConditions run5</ImagePath>
        <Description>Provides storage, processing and controlled access of data and rapid transaction processing.</Description>
        <DisplayName>SQL Server (SQLEXPRESS)</DisplayName>
        <Startup>Automatic</Startup>
        <StartOnDeploy>true</StartOnDeploy>
        <LogOnAs>
          <Username>NT AUTHORITY\NetworkService</Username>
        </LogOnAs>
        <Timeout>300000</Timeout>
      </Service>
    ```
  + **Shortcuts\.xml** — Contains instructions to configure any program shortcuts captured during packaging\. The `<Target>` is the package run program with a run condition as argument\. The run condition is detailed in the `Programs.xml` file\. When a program is launched from a shortcut, it runs as a child process of the package engine\. 

    ```
    <Shortcuts xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Shortcut>
        <Path>%CommonPrograms%\Microsoft SQL Server 2005\SQL Server Management Studio Express.lnk</Path>
        <Target>%DefaultDir%\Compatibility.Package.Run.exe</Target>
        <Args>/RunConditions run1</Args>
        <Description />
        <IconLocation>%DefaultDir%\ProgData\Program Files (x86)\Microsoft SQL Server\90\Tools\Binn\VSShell\Common7\IDE\ssmsee.exe</IconLocation>
        <IconIndex>0</IconIndex>
        <WorkingDir />
      </Shortcut>
    ```
  + **eula\.html** — The end\-user license agreement file\.
  + **Open Source Licenses\.txt ** — A license agreement that covers the open source components\.

## Deployed package contents<a name="emp-package-contents-deployed"></a>

When the source package is deployed to the target Windows server, the contents of the `.\EngineBinaries\x64` or `.\EngineBinaries\x86` folders are copied into the package root folder during the package deployment depending on the bit rate of the operating system\. The `DeploymentWorkFlowLog.txt` is populated with the logged package deployment events\. 

Once the package has been launched by the start of a service, when an application shortcut or other entry point into the package is launched, another log file is created within the deployed package root folder called `RunWorkFlowLog.txt`\.

**RunWorkFlowLog\.txt**  
Contains a brief log of events that take place from package invocation to the launch of the package engine\. Once the `RunCondition` is passed to the package engine that has successfully launched, package run exits after logging a successful launch event\. 