# Deploying an ASP\.NET Web App to AWS<a name="tutorial-eb"></a>

The following tutorial demonstrates how to use the *AWS Elastic Beanstalk Deployment* task to deploy a web application to the AWS Cloud from a Visual Studio Team Services \(VSTS\) build definition\.

## Prerequisites<a name="prerequisites"></a>
+ The AWS Tools for VSTS installed in VSTS or an on\-premises Team Foundation Server\.
+ An AWS account and preferably an associated IAM user account\.
+ An Elastic Beanstalk application and environment\.

## Deploying an ASP\.NET Application Using the AWS Elastic Beanstalk Deployment Task<a name="deploying-an-asp-net-application-using-the-aeblong-deployment-task"></a>

This walkthrough assumes you are using a build based on the ASP\.NET Core \(\.NET Framework\) template that will produce a Web Deploy archive for deployment\.

![\[Select a template\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/choose-template.png)

The build process page containing the default tasks for the template is displayed\.

![\[Build Definition\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/build-definition.png)

### Add the AWS Elastic Beanstalk Deployment Task to the Build Definition<a name="add-the-aws-elastic-beanstalk-deployment-task-to-the-build-definition"></a>

Click the **Add Task** link\. In the right hand panel, scroll through the available tasks until you see the *AWS Elastic Beanstalk Deployment* task\. Click the **Add** button to add it to bottom of the build definition\.

![\[AWS Elastic Beanstalk Deployment Task\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/elastic-beanstalk-task-in-list.png)

Click on the new task and you will see the properties for it in the right pane\.

![\[AWS Elastic Beanstalk Deployment Task in Position\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/build-process-list-eb.png)

### Configure the Task Properties<a name="configure-the-task-properties"></a>

For the new task you need to make the following configuration changes\.
+ AWS Credentials: If you have existing AWS credentials configured for your tasks you can select them using the dropdown link in the field\. If not, to quickly add credentials for this task, click the **\+** link\.  
![\[AWS Credential Field\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialsfield.png)

  This opens the **Add new AWS Connection** form\.  
![\[AWS Credential Dialog\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialdialog.png)

  This task requires credentials for a user with a policy enabling the user to update a Beanstalk environment and describe an environment status and events\. Enter the access key and secret keys for the credentials you want to use and assign a name that you will remember\.
**Note**  
We recommend that you do not use your account's root credentials\. Instead, create one or more IAM users, and then use those credentials\. For more information, see [Best Practices for Managing AWS Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

  Click **OK** to save them\. The dialog will close and return to the Elastic Beanstalk Deployment Task configuration with the new credentials selected\.  
![\[AWS Credential Dialog\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialssavedEB.png)
+ AWS Region: The AWS region that that the Beanstalk environment is running in\.
+ Application Type: Set to ASP\.NET
+ Web Deploy Archive: The path to the Web Deploy archive\. If the archive was created using the arguments above, the file will have the same name as the directory containing the web application and will have a "\.zip" extension\. It can be found in the build artifacts staging directory which can be referenced as `$(build.artifactstagingdirectory)`\.
+ Application Name: The name you used to create the Beanstalk application\. A Beanstalk application is the container for the environment for the \.NET web application\.
+ Environment Name: The name you used to create the Beanstalk environment\. A Beanstalk environment contains the actual provisioned resources that are running the \.NET web application\.

### Run the Build<a name="run-the-build"></a>

With the new task configured you are ready to run the build\. Click the Save and queue option\. When the build has completed running you should see a log similar to this\.

![\[Build Log\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/build-succeeded-log.png)