# High\-level AWS End\-of\-Support Migration Program \(EMP\) for Windows Server application discovery exercise<a name="emp-high-level-discovery"></a>

After you identify the applications to migrate using EMP, we recommend that you perform an application discovery exercise for each application\. EMP application discovery refers to the process of gathering and documenting all of the information about a legacy application that is required to inform the creation of a functioning EMP package and its deployment onto a modern, supported target environment\. The information required to complete application discovery comes from both the legacy and target environments\. It typically includes configuration details, installation instructions, topology, customizations, security requirements, users, and more\.

When the application discovery is complete, analyze the information against the [EMP limitations](emp-limitations.md) to determine EMP eligibility\. 

We recommend that an application subject matter expert \(SME\) is available to assist with the discovery process\. The application SME is a representative who knows how the application works for the business and is familiar with the business workflows within the application\. The SME can demonstrate all business workflows and also define or agree upon an acceptance testing plan for the application if one does not exist\. The acceptance testing plan can be used during the testing of the application in the EMP package\.

The following table shows a discovery checklist to guide you through the discovery process\.


| Checklist item | Details | 
| --- | --- | 
| Application name and version |  The name and version of the application that you want to migrate\.  | 
| Application subject matter expert \(SME\) | The name and contact details of the application SME to assist with the migration\. | 
| Software prerequisites required by the application | The software required to be installed before or with the application\. For example, \.NET 2\.0 SP1, earlier versions of Java, Visual C\+\+ runtimes, and more\. The dependencies can be included in the same package as the application\. | 
| Source operating system | The operating system on which the application currently runs\. | 
| Target operating system | The operating system to which you want to migrate the application\. | 
| Are the install media and install instructions available? | The answer to this question determines the EMP packaging model to be followed for migration \([standard](emp-getting-started-packaging-media.md) or [GRP](emp-getting-started-packaging-guided-reverse.md) packaging\)\. For standard packaging, gather install media, instructions, and customizations required to carry out a complete setup of the application on the legacy OS\.  | 
| Application topology and external dependencies | For example, three\-tier topology with an application/desktop tier, an IIS\-based web tier, and a database tier all hosted on different servers requiring connectivity between one another\. | 
| Does the application require specific drivers? | If so, is a 64\-bit compatible version available? \(Check the [EMP limitations](emp-limitations.md)\)\. | 
| Other application details | What is the bitrate of the application: 32\-bit or 64\-bit? Does it use COM\+ or DCOM components? Are Windows Services installed or used by the application? Do any services require domain or local service accounts? Is the application subject to Data Execution Prevention \(DEP\)? Are there any firewall ports that should be opened? | 
| Windows features and roles required | The Windows features and roles required by the application to set up on the modern operating system\. | 
| Known issues | Any known issues of the application to account for during the testing phases\. | 
| Cross\-check against EMP limitations | Verify whether the application is in scope for EMP packaging\. | 