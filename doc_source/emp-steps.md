# The AWS End\-of\-Support Migration Program \(EMP\) for Windows Server process<a name="emp-steps"></a>

This section provides a high\-level overview of the EMP process, including the steps involved in each phase of the process\.

![\[Overview of the EMP process.\]](http://docs.aws.amazon.com/emp/latest/userguide/images/emp-process-2-b.png)

**Topics**
+ [Discovery](#emp-steps-discovery)
+ [Packaging preparation](#emp-steps-packaging-prep)
+ [Packaging](#emp-steps-packaging)
+ [Deployment](#emp-steps-deployment)

## Discovery<a name="emp-steps-discovery"></a>

During discovery, target applications are recognized using AWS Optimization and Licensing Assessment \(AWS OLA\)\. Discovery includes identifying legacy applications that require EMP packaging for migration to later versions of the Windows Server operating system\.

## Packaging preparation<a name="emp-steps-packaging-prep"></a>

Packaging preparation includes the following steps\.

1. Clone the server image to the AWS instance, if possible\.

1. Complete application discovery\.

1. Plan user acceptance testing \(UAT\)\.

1. Define the target environment\.

## Packaging<a name="emp-steps-packaging"></a>

Packaging includes the following steps\.

1. EMP package builder packages the application on the source server version, guided by application discovery\. The source server version is the Windows Server operating system that currently hosts the application\.

1. The package is tested on a clean source server version and any required remediation is performed\.

1. The package is tested on the target server version instance following the UAT plan\. Any necessary remediation is performed\.

## Deployment<a name="emp-steps-deployment"></a>

Deployment includes the following steps\.

1. Deployment and testing on the target environment\.

1. Production deployment\.