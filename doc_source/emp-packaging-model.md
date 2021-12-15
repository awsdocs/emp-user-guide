# AWS End\-of\-Support Migration Program \(EMP\) for Windows Server application packaging model<a name="emp-packaging-model"></a>

The following model and descriptions show the process of packaging a legacy application using AWS End\-of\-Support Migration Program \(EMP\) for Windows Server\. There are two packaging scenarios:
+ **Standard packaging**\. Use when the application media and install instructions are available to recreate an up\-to\-date installation of a legacy application in its current supported operating system\.
+ **Reverse packaging**\. Use when the application media or the install instructions are not available to recreate an up\-to\-date installation of a legacy application in its current supported operating system\. 

**Note**  
If the criteria for standard packaging is met, then we recommend that you apply this packaging method instead of the reverse packaging method\. 

![\[Overview of the EMP packaging model.\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-application-packaging-model-2.png)

**Application server**  
The legacy application server on which the legacy application is installed and runs\.

**Clone of the application server**  
The clone of the application server that is used for the process capture phase of the reverse packaging process\. For information about the reverse packaging process, see [How to package an application when installation media is not available \(reverse packaging\)](emp-getting-started-packaging-no-media.md)\.

If you do not want to clone the production servers, then you can skip this step and follow the initial steps of the reverse packaging process on the production application server\. We recommend that you follow standard best practices for working on a production system\.

**Packaging server**  
The server is an original build, including service packs, of the server operating system where the application is installed and runs\. The EMP product is installed and used to create the EMP application package on this server\. 

**Testing Server—source operating system**  
This testing server is an original build, including service packs, of the server operating system where the application is installed and runs\. It is used to validate the EMP package on the operating system on which it was created\.

**AWS Testing Server—deployment operating system**  
This testing server is the target operating system on which the EMP package must run\. This server is used to validate the application in an EMP package on the target operating system within the AWS environment\.

**On\-premises testing server—deployment operating system**  
This server is the target operating system on which the EMP packages must run\. This server is used to validate the application in an EMP package in the target, on\-premises operating system\.

**Note**  
This step is required only if there is a benefit to test the package in an on\-premises environment to validate that it works as expected before migrating the application to AWS, or if it is required to troubleshoot any issues\. It helps identify whether issues are the result of the AWS environment setup, the EMP package, or the target operating system\.

If you want someone at AWS or one of our partners to perform the application packaging for you, then submit a request to [AWS IQ](https://iq.aws.amazon.com/?utm=docs)\.

**Topics**
+ [Standard packaging](emp-getting-started-packaging-media.md)
+ [Reverse packaging](emp-getting-started-packaging-no-media.md)