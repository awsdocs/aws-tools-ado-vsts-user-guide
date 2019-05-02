# Getting Started<a name="getting-started"></a>

This section provides information about how to install, set up, and use the AWS Tools for Microsoft Visual Studio Team Services\.

## Set up a VSTS Account<a name="set-up-a-vsts-account"></a>

To use [Visual Studio Team Services \(VSTS\)](https://www.visualstudio.com/team-services/), you need to sign up for a [Visual Studio Team Services Account](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)\.

## Install the AWS Tools for VSTS Extension<a name="install-the-aws-tools-for-vsts-extension"></a>

The AWS Tools for VSTS is installed from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-vsts-tools)\. Sign in to your VSTS account, then search for *AWS Tools for Microsoft Visual Studio Team Services*\. Choose **Install** to download to VSTS, or choose **Download** to install into an on\-premises Team Foundation Server\.

![\[Download Team Services Extension\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/AWSVSTSdownload.png)<a name="setup-credentials"></a>

## Set up AWS Credentials for the AWS Tools for VSTS<a name="set-up-aws-credentials-for-the-aws-tools-for-vsts"></a>

To use the AWS Tools for VSTS to access AWS, you need an AWS account and AWS credentials\. When build agents run the tasks contained in the tools the tasks must be configured with, or have access to, those AWS credentials to enable them to call AWS service APIs\. To increase the security of your AWS account, we recommend that you use an *IAM user* to provide access credentials to the tasks running in the build agent processes instead of using your root account credentials\.

**Note**  
For an overview of IAM users and why they are important for the security of your account, see [Overview of Identity Management: Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html) in the *IAM User Guide*\.

**To sign up for an AWS account**

1. Open [http://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Sign Up**\.

1. Follow the onscreen instructions\. Part of the signup procedure involves receiving a phone call and entering a PIN using your phone keypad\.

Next, create an IAM user and download \(or copy\) its secret access key\. To use the AWS Tools for VSTS, you must have a set of valid AWS credentials, which consist of an access key and a secret key\. These keys are used to sign programmatic web service requests and enable AWS to verify that the request comes from an authorized source\. You can obtain a set of account credentials when you create your account\. However, we recommend that you do not use these credentials with AWS Tools for VSTS\. Instead, [create one or more IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html), and use those credentials\.

**To create an IAM user**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home) \(you may need to sign in to AWS first\)\.

1. Choose **Users** in the sidebar to view your IAM users\.

1. If you don't have any IAM users set up, choose **Create New Users** to create one\.

1. Select the IAM user in the list that you want to use to access AWS\.

1. Open the **Security Credentials** tab, and then choose **Create Access Key**\.
**Note**  
You can have a maximum of two active access keys for any given IAM user\. If your IAM user has two access keys already, you need to delete one of them before creating a new key\.

1. In the dialog box that opens, choose **Download Credentials** to download the credential file to your computer\. Or choose **Show User Security Credentials** to view the IAM user's access key ID and secret access key \(which you can copy and paste\)\.
**Important**  
There is no way to obtain the secret access key once you close the dialog box\. You can, however, delete its associated access key ID and create a new one\.

Once an AWS account has been created, and preferably an IAM user for that account as detailed above, you can supply credentials to the tasks in a number of ways:
+ By configuring a service endpoint, of type *AWS*, and referencing that endpoint when configuring tasks\.
+ By creating specific named variables in your build\. The variable names for supplying credentials are *AWS\.AccessKeyID*, *AWS\.SecretAccessKey* and optionally *AWS\.SessionToken*\. To pass the region in which the AWS service API calls should be made you can also specify *AWS\.Region* with the region code \(eg *us\-west\-2*\) of the region\.
+ By using standard AWS environment variables in the build agent process\. These variables are *AWS\_ACCESS\_KEY\_ID*, *AWS\_SECRET\_ACCESS\_KEY* and optionally *AWS\_SESSION\_TOKEN*\. To pass the region in which the AWS service API calls should be made you can also specify *AWS\_REGION* with the region code \(eg *us\-west\-2*\) of the region\.
+ For build agents running on Amazon EC2 instances the tasks can automatically obtain credential and region information from instance metadata associated with the EC2 instance\. For credentials to be available from EC2 instance metadata the instance must have been started with an instance profile referencing a role granting permissions to the task to make calls to AWS on your behalf\. See [IAMRolesForEC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) for more information\.

## Supplying Task Credentials using a Service Endpoint<a name="supplying-task-credentials-using-a-service-endpoint"></a>

If you choose to use service endpoints, of type *AWS*, to convey credentials to the AWS tasks in the tools you can link your AWS subscription from the **Services** tab in the Account Administration section for a project\. Note that a service endpoint expects long\-lived AWS credentials consisting of an access and secret key pair, to be provided\. Alternatively you can define *Assume Role* credentials\. Service endpoints do not support the use of a session token variable\. To use these forms of temporary AWS credentials use the build or environment variable approaches as defined earlier, or run your build agents on Amazon EC2 instances\.

To link the AWS subscription to use in Build or Release Management definitions, open the Account Administration page \(choose the gear icon on the top right of the page\), and then choose **Services**\. Choose **\+ New Service Endpoint**\. Select the **AWS** endpoint type\. This opens the **Add new AWS Connection** form\.

![\[Create an AWS endpoint\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/AddNewAWSConnection.png)

Provide the following parameters, and then click **OK**:
+ Connection name
+ Access key ID
+ Secret access key

The connection name is used to refer to these credentials when you are configuring tasks that access this set of AWS credentials in your build and release definitions\.

For more information, see [About Access Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html?icmpid=docs_iam_console)\.