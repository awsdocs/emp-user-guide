# AWS End\-of\-Support Migration Program \(EMP\) for Windows Server application packaging model<a name="emp-packaging-model"></a>

The following model and descriptions show the process of packaging a legacy application using AWS End\-of\-Support Migration Program \(EMP\) for Windows Server\. There are two packaging models:
+ **Standard packaging**\. Use when the application media and install instructions are available to recreate an up\-to\-date installation of a legacy application in its current supported operating system\.
**Note**  
Standard packaging supports packaging applications that are installed only on a single system drive \(typically, the `C:` drive\)\.
+ **Guided Reverse Packaging \(GRP\)**\. Use when the application media and install instructions are not available to recreate an up\-to\-date installation of a legacy application in its current supported operating system\.

**Note**  
If the criteria for standard packaging is met, then we recommend that you apply this packaging model instead of the GRP packaging model\. 

![\[Overview of the EMP packaging model.\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-application-packaging-model-3.png)

**Application server**  
The legacy application server on which the legacy application is installed and runs\.

**Clone of the application server**  
Guided Reverse Packaging \(GRP\) is performed directly on the clone of the source server\. For more information about GRP, see [How to package an application when installation media is not available \(Guided Reverse Packaging\)](emp-getting-started-packaging-guided-reverse.md)\. 

If you do not want to clone the production servers, then you can skip this step and continue with GRP\. We recommend that you follow standard best practices for working on a production system\.

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
+ [Guided Reverse Packaging](emp-getting-started-packaging-guided-reverse.md)