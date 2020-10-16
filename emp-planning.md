# Planning an AWS End\-of\-Support Migration Program \(EMP\) for Windows Server migration<a name="emp-planning"></a>

When you plan an AWS End\-of\-Support Migration Program \(EMP\) for Windows Server migration, consider the following:
+ **Application environment — legacy to target **

  It is important to understand the existing architecture of the application that is being migrated so that the target environment can be correctly set up\. In an n\-tier application model, you should identify each server that makes up the model and understand which of the servers require migration and which of them require EMP for the migration process\. Some of the servers may already be migrated onto a modern platform in AWS and using a step\-by\-step migration approach ensures a smooth transition to AWS\. 

  For example, if you attempt to migrate a three\-tier application that consists of an Internet Information Server \(IIS\) web server, application server, and SQL Server 2008, all running on Windows Server 2008 on premises, the migration plan might look like this: the IIS Web Server migrates to a Windows Server 2019 in AWS without the need for EMP migration\. The application running on the application server is captured into an EMP package and migrated onto a Windows Server 2019 running on AWS, and SQL Server is migrated to Amazon Relational Database Service \(Amazon RDS\)\. If the application works only on the legacy version of SQL Server, then it can be captured into an EMP package and migrated to a modern operating system\.
+ **Standard vs\. reverse packaging**

  The [application discovery](emp-high-level-discovery.md) exercise determines whether the [standard](emp-getting-started-packaging-media.md) or [reverse](emp-getting-started-packaging-no-media.md) packaging model should be applied\.
**Note**  
If the criteria for standard packaging is met, we recommend that you follow this methodology rather than the reverse packaging methodology\.
+ **Packaging and testing environment**

  Set up your packaging and testing environments according to the results of the application discovery exercise\. For more information about each packaging scenario, see [AWS End\-of\-Support Migration Program \(EMP\) for Windows Server application packaging model](emp-packaging-model.md)\. 
