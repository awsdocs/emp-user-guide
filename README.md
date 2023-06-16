# NOTICE

**This repository is archived and the content on this branch is out of date.** You can find up-to-date AWS technical documentation on the [AWS Documentation website](https://docs.aws.amazon.com/), where you can also submit feedback and suggestions for improvement.

# Announcement

This repository will be archived and marked read-only next month (June 2023). For more information, read [the announcement on the AWS News Blog](https://aws.amazon.com/blogs/aws/retiring-the-aws-documentation-on-github/).

You can find the corresponding content for this repo on [the AWS Documentation website](https://docs.aws.amazon.com/emp/latest/userguide). If you'd like to continue contributing to the quality of AWS documentation, you can submit feedback and suggestions for improvement there.

# AWS End-of-Support Migration Program (EMP) for Windows Server User Guide
-----

The End-of-Support Migration Program for Windows Server (EMP) migrates legacy applications from Windows Server 2003, 2008, and 2008 R2 to newer, supported versions on AWS without any code refactoring. The EMP technology decouples the application from the existing operating system and packages it with all necessary application files and components. The program then replatforms it to a supported version of the Windows operating system on AWS. After the migration, any outdated API calls are redirected, enabling the application to run on the latest versions of the Windows OS.


-----
*****Copyright &copy; 2020 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.



## Contents
+ [What Is AWS End-of-Support Migration Program (EMP) for Windows Server?](doc_source/emp-what-is.md)
+ [How AWS End-of-Support Migration Program (EMP) for Windows Server works](doc_source/emp-how-it-works.md)
   + [The AWS End-of-Support Migration Program (EMP) for Windows Server process](doc_source/emp-steps.md)
   + [AWS End-of-Support Migration Program (EMP) for Windows Server System requirements](doc_source/emp-supported-os.md)
   + [AWS End-of-Support Migration Program (EMP) for Windows Server limitations](doc_source/emp-limitations.md)
   + [Data collection](emp-data.md)
+ [Get started with AWS End-of-Support Migration Program (EMP) for Windows Server](doc_source/emp-getting-started.md)
   + [AWS End-of-Support Migration Program (EMP) for Windows Server decision tree](doc_source/emp-decision-tree.md)
   + [Planning an AWS End-of-Support Migration Program (EMP) for Windows Server migration](doc_source/emp-planning.md)
   + [High-level AWS End-of-Support Migration Program (EMP) for Windows Server application discovery exercise](doc_source/emp-high-level-discovery.md)
   + [Install AWS EMP Compatibility Package Builder](emp-install-compatibility-package-builder.md)
   + [AWS End-of-Support Migration Program (EMP) for Windows Server application packaging model](doc_source/emp-packaging-model.md)
      + [How to package an application when installation media is available (standard packaging)](doc_source/emp-getting-started-packaging-media.md)
      + [How to package an application when installation media is not available (reverse packaging)](doc_source/emp-getting-started-packaging-no-media.md)
   + [Deploy an EMP package](emp-deploy.md)
+ [Manage EMP packages](emp-manage.md)
   + [Best practices for packaging applications with AWS End-of-Support Migration Program (EMP) for Windows Server](doc_source/emp-best-practices.md)
   + [EMP compatibility package features](doc_source/emp-compatibility-package-features.md)
   + [Edit, upgrade, and maintain an EMP package](doc_source/emp-edit-upgrade-maintain.md)
   + [Update a deployed EMP package](doc_source/emp-deploy-updated-package.md)
   + [Uninstall an EMP package](doc_source/emp-uninstall.md)
+ [Security in AWS End-of-Support Migration Program (EMP) for Windows Server](doc_source/emp-security.md)
   + [Data collected by the AWS End-of-Support Migration Program (EMP) for Windows Server](doc_source/emp-security-data.md)
+ [Troubleshooting AWS End-of-Support Migration Program (EMP) for Windows Server](doc_source/emp-troubleshooting.md)
+ [AWS End-of-Support Migration Program (EMP) for Windows Server version history](doc_source/emp-versions.md)
+ [Document History for User Guide](doc_source/doc-history.md)
