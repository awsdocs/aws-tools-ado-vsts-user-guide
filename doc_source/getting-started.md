# Getting Started<a name="getting-started"></a>

This section provides information about how to install, set up, and use the AWS Tools for Microsoft Visual Studio Team Services\.

## Set up a VSTS Account<a name="set-up-a-vsts-account"></a>

To use [Visual Studio Team Services \(VSTS\)](https://visualstudio.microsoft.com/team-services/), you will first need to [sign up for a Visual Studio Team Services Account](https://docs.microsoft.com/en-us/azure/devops/user-guide/sign-up-invite-teammates?view=azure-devops)\.

## Install the AWS Tools for VSTS Extension<a name="install-the-aws-tools-for-vsts-extension"></a>

Go to the relevant [Visual Studio Marketplace](https://marketplace.visualstudio.com/) and search for *AWS Tools for Microsoft Visual Studio Team Services*\. \(The following URL is a direct link to the AWS Tools for VSTS: [https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-vsts-tools](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-vsts-tools)\.\)

Choose **Get it free** and sign in to your VSTS account if prompted\. Then choose **Install** to install into your VSTS account, or choose **Download** to install into an on\-premises server\.

![\[Download Team Services Extension\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/AWSVSTSdownload.png)<a name="setup-credentials"></a>

## Establish AWS Credentials for the AWS Tools for VSTS<a name="set-up-aws-credentials-for-the-aws-tools-for-vsts"></a>

To use the AWS Tools for VSTS to access AWS, you need an AWS account and AWS credentials\. When build agents run the tasks contained in the tools, the tasks must be configured with, or have access to, those AWS credentials to enable them to call AWS service APIs\. To increase the security of your AWS account, we recommend that you do not use your root account credentials, but rather create an *IAM user* to provide access credentials to the tasks running in the build agent processes\.

**Note**  
For an overview of IAM users and why they are important for the security of your account, see [Overview of Identity Management: Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html) in the *IAM User Guide*\.

**Sign Up for an AWS Account**

1. Open [http://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.

1. Follow the onscreen instructions\. Part of the signup procedure involves receiving a phone call and entering a PIN using your phone keypad\.

**Create an IAM User and Download Its Credentials**

Next, create an IAM user and download \(or copy\) its credentials\. To use the AWS Tools for VSTS, you must have a set of valid AWS credentials, which consist of an access key and a secret key\. These keys are used to sign programmatic web service requests and enable AWS to verify that the request comes from an authorized source\. You can obtain a set of account credentials when you create your account\. However, we recommend that you do not use these credentials with AWS Tools for VSTS\. Instead, [create one or more IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html), and use those credentials\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/home) \(you may need to sign in to AWS first\)\.

1. Choose **Users** in the sidebar to view your IAM users\.

1. If you don't have any IAM users set up, choose **Create New Users** to create one\.

1. Select the IAM user in the list that you want to use to access AWS\.

1. Open the **Security Credentials** tab, and then choose **Create Access Key**\.
**Note**  
You can have a maximum of two active access keys for any given IAM user\. If your IAM user has two access keys already, you need to delete one of them before creating a new key\.

1. In the **Create access key** dialog box that opens, choose **Download \.csv file** to download the credential file to your computer\. Or choose the **Show** link in the **Secret access key** column to view the IAM user's secret access key, and then copy the access key ID and the secret access key\.
**Important**  
There is no way to obtain the secret access key once you close the dialog box\. You can, however, delete its associated access key ID and create a new one\.

## Supplying Task Credentials \(Overview\)<a name="supplying-task-creds-overview"></a>

Once an AWS account has been created, and preferably an IAM user for that account as detailed above, you can supply credentials to the tasks in a number of ways:
+ By configuring a service endpoint of type *AWS* and referencing that endpoint when configuring tasks\.
+ By creating specific named variables in your build\. The variable names for supplying credentials are *AWS\.AccessKeyID*, *AWS\.SecretAccessKey* and optionally *AWS\.SessionToken*\. To pass the region in which the AWS service API calls should be made you can also specify *AWS\.Region* with the region code \(for example, *us\-west\-2*\) of the region\.
+ By using standard AWS environment variables in the build agent process\. These variables are *AWS\_ACCESS\_KEY\_ID*, *AWS\_SECRET\_ACCESS\_KEY* and optionally *AWS\_SESSION\_TOKEN*\. To pass the region in which the AWS service API calls should be made you can also specify *AWS\_REGION* with the region code \(for example, *us\-west\-2*\) of the region\.
+ For build agents running on Amazon EC2 instances the tasks can automatically obtain credential and region information from instance metadata associated with the EC2 instance\. For credentials to be available from EC2 instance metadata the instance must have been started with an instance profile referencing a role granting permissions to the task to make calls to AWS on your behalf\. See [IAMRolesForEC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) for more information\.

## Supplying Task Credentials using a Service Endpoint<a name="supplying-task-credentials-using-a-service-endpoint"></a>

If you choose to use service endpoints of type *AWS* to convey credentials to the AWS tasks in the tools, you can create a link to your AWS subscription by using the **Service connections** section of the **Project settings** for your project\. Note that a service endpoint expects long\-lived AWS credentials consisting of an access\-key and secret\-key pair\. Alternatively you can define *Assume Role* credentials\. Service endpoints do not support the use of a session token variable\. To use these forms of temporary AWS credentials, use the build or environment variable approaches as defined earlier, or run your build agents on Amazon EC2 instances\.

To create a link to the AWS subscription for use in Build or Release Management definitions:

1. Find the gear icon for your project, either in the lower, left\-hand corner of the window or by hovering over the project's name\.

1. Choose the icon to open the **Project settings** for your project\.

1. Choose **Service connections**\.

1. Under **\+ New service connection**, select the **AWS** endpoint type\. This opens the **Add AWS service connection** form\.  
![\[Create an AWS endpoint\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/AddNewAWSConnection.png)

1. Provide the following parameters, and then click **OK**:
   + Connection name
   + Access key ID
   + Secret access key

   The connection name is used to refer to these credentials when you are configuring tasks that access this set of AWS credentials in your build and release definitions\.

For more information, see [About Access Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html?icmpid=docs_iam_console) in the *[IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)*\.