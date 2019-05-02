# Task Reference<a name="task-reference"></a>

This reference describes the tasks that are included in the AWS Tools for Microsoft Visual Studio Team Services\.<a name="task-prerequisites"></a>

 **Prerequisites** 

You must have an AWS account\. For information on setting up an account, see [AWSSignUp](https://docs.aws.amazon.com/sdk-for-net/v3/ndg/gs-signup-for-aws.html)\.

Each task requires AWS credentials for your account be available to the build agent running your task, and the region in which the API calls to AWS services should be made\.

You can either:
+ Specify credentials explicitly for each task, by configuring a named service endpoint \(of endpoint type *AWS*\) and then referring to the endpoint name in the *AWS Credentials* field for each task\. Region can be set in the *AWS Region* property for a task\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/AddNewAWSConnection.png)  
alt  
Add an AWS connection
+ Supply credentials and region to tasks using environment variables in the process hosting the build agent\.
+ If your build agent is running on an Amazon EC2 instance you can also elect to have credentials \(and region\) be obtained automatically from the instance metadata associated with the instance\. For credentials to be available from EC2 instance metadata the instance must have been started with an instance profile referencing a role granting permissions to the task to make calls to AWS on your behalf\. See [Using an IAM Role to Grant Permissions to Applications Running on Amazon EC2 Instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) for more information

 **Note:** If you choose to use an AWS Service Endpoint to supply credentials to tasks we strongly recommend using an AWS Identity and Access Management user account, with appropriate permissions to scope the privileges of the user account to only those needed to execute the task\(s\) you need\.

**Topics**
+ [AWS CLI](aws-cli.md)
+ [AWS Tools for Windows PowerShell Script Task](awspowershell-module-script.md)
+ [AWS Shell](awsshell.md)
+ [AWS CloudFormation Create\-Update Stack Task](cloudformation-create-update.md)
+ [AWS CloudFormation Delete Stack Task](cloudformation-delete-stack.md)
+ [AWS CloudFormation Execute Change Set Task](cloudformation-execute-changeset.md)
+ [AWS CodeDeploy Deployment Application Task](codedeploy-deployment.md)
+ [Amazon Elastic Container Registry Push Image](ecr-pushimage.md)
+ [AWS Elastic Beanstalk Create Version Task](elastic-beanstalk-createversion.md)
+ [AWS Elastic Beanstalk Deployment Task](elastic-beanstalk-deploy.md)
+ [AWS Lambda Deployment Task](lambda-deploy.md)
+ [AWS Lambda Invoke Function Task](lambda-invoke.md)
+ [AWS Lambda \.NET Core Deployment Task](lambda-netcore-deploy.md)
+ [AWS S3 Download Task](s3-download.md)
+ [AWS S3 Upload Task](s3-upload.md)
+ [AWS Secrets Manager Create/Update Secret](secretsmanager-create-update.md)
+ [AWS Secrets Manager Get Secret](secretsmanager-getsecret.md)
+ [AWS Send Message Task](send-message.md)
+ [AWS Systems Manager Get Parameter](systemsmanager-getparameter.md)
+ [AWS Systems Manager Set Parameter](systemsmanager-setparameter.md)
+ [AWS Systems Manager Run Command](systemsmanager-runcommand.md)