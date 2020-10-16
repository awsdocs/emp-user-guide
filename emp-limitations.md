# AWS End\-of\-Support Migration Program \(EMP\) for Windows Server limitations<a name="emp-limitations"></a>

 The following application and component types cannot be migrated using AWS End\-of\-Support Migration Program \(EMP\) for Windows Server\. 
+ Applications that do not run on the Windows operating system\.
+ 16\-bit applications\. If the target operating system is a 64\-bit Windows operating system, the NT Virtual DOS Machine \(NTVDM\) required to run these applications is available on 32\-bit Windows operating systems only\.
+ Kernel\-mode drivers that are a different bitness than the target operating system\. Device drivers are not virtualized with EMP and therefore must be compatible with the target operating system\-compatible drivers to be deployed with the package\. For example, if you are moving to a 64\-bit operating system, you must have a 64\-bit driver that is compatible with the new operating system\.
+ Low\-level applications\. For example, antivirus, firewall, and VPN applications\.
+ Explorer Shell Extensions\.
+ Microsoft BizTalk and Microsoft Transaction Server \(MTS\)\-based systems\.
+ Desktop applications\.
