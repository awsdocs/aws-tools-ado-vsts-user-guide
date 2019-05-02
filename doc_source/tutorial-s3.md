# Archiving Build Artifacts to AWS<a name="tutorial-s3"></a>

The following tutorial demonstrates how to use the *AWS S3 Upload* task to upload archival data to an Amazon Simple Storage Service \(Amazon S3\) bucket from a Visual Studio Team Services \(VSTS\) build definition\.

## Prerequisites<a name="prerequisites"></a>
+ The AWS Tools for VSTS installed in VSTS or an on\-premises Team Foundation Server\.
+ An AWS account and preferably an associated IAM user account\.
+ An S3 bucket\.

## Archiving Build Artifacts with the AWS S3 Upload Task<a name="archiving-build-artifacts-with-the-aws-s3-upload-task"></a>

This walkthrough assumes you are using a build based on the ASP\.NET Core template which contains the following default tasks\.

![\[New build pipeline\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/startingbuilddefinition.png)

### Add the S3 Upload Task to the Build Definition<a name="add-the-s3-upload-task-to-the-build-definition"></a>

To capture the build output produced by the *Publish* task and upload it to Amazon S3 you need to add the *AWS S3 Upload* task between the existing *Publish* and *Publish Artifacts* tasks\. Click the **Add Task** link\. In the right hand panel, scroll through the available tasks until you see the AWS S3 Upload task\. Click the **Add** button to add it to the build definition\.

![\[AWS S3 Upload Task\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/tasklist.png)

If the new task is not added immediately after the *Publish* task, drag and drop it into position\.

![\[AWS S3 Upload Task in Position\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/s3taskstart.png)

Click on the new task and you will see the properties for it in the right pane\.

### Configure the Task Properties<a name="configure-the-task-properties"></a>

For the new task you need to make the following configurations changes\.
+ AWS Credentials: If you have existing AWS credentials configured for your tasks you can select them using the dropdown link in the field\. If not, to quickly add credentials for this task, click the **\+** link\.  
![\[AWS Credential Field\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialsfield.png)

  This opens the **Add new AWS Connection** form\.  
![\[AWS Credential Dialog\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialdialog.png)

  This task requires credentials for a user with a policy enabling the user to put objects to S3\. If the create S3 bucket option is enabled you also need permission to create a bucket\. Enter the access key and secret keys for the credentials you want to use and assign a name that you will remember\.
**Note**  
We recommend that you do not use your account's root credentials\. Instead, create one or more IAM users, and then use those credentials\. For more information, see [Best Practices for Managing AWS Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

  Click **OK** to save them\. The dialog will close and return to the AWS S3 Upload Task configuration with the new credentials selected\.  
![\[AWS Credential Dialog\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/credentialssavedS3.png)
+ Set the region in which the bucket exists or will be created in, for example 'us\-east\-1', 'us\-west\-2' etc\.
+ Enter the name of the bucket \(bucket names must be globally unique\)\.
+ The **Source Folder** points to a folder in your build area that contains the content to be uploaded\. Team Services provides a number of variables, detailed here, that you can use to avoid hard\-coded paths\. For this walk\-through use the variable **Build\.ArtifactStagingDirectory**, which is defined as *the local path on the agent where artifacts are copied to before being pushed to their destination*\.
+  **Filename Patterns** can contain one or more globbing patterns used to select files under the **Source Folder** for upload\. The default value shown here selects all files recursively\. Multiple patterns can be specified, one per line\. For this walk\-through, the preceeding task \(*Publish*\) emits a zip file containing the build which is the file that will be uploaded\.
+  **Target Folder** is the *key prefix* in the bucket that will be applied to all of the uploaded files\. You can think of this as a folder path\. If no value is given the files are uploaded to the root of the bucket\. Note that by default the relative folder hierarchy is preserved\.
+   
**There are 3 additional options that can be set:**  
  + Create S3 bucket if it does not exist\. The task will fail if the bucket cannot be created\.
  + Overwrite \(in the Advanced section\) \- this is selected by default\.
  + Flatten folders \(also in Advanced section\)\. Flattens the folder structure and copies all files into the specified target folder in the bucket, removing their relative paths to the source folder\.

### Run the Build<a name="run-the-build"></a>

With the new task configured you are ready to run the build\. Click the Save and queue option\.

![\[Save and Queue the Build\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/s3taskfinal2.png)

During the build you can view the log by clicking on the build number in the queue message\.

![\[Save and Queue the Build\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/click-on-build-number-to-view-log.png)

When the build has completed you will be able to see the S3 upload logs\.

![\[Task Log\]](http://docs.aws.amazon.com/vsts/latest/userguide/images/tasklog.png)

That completes the walk\-through\. As you have seen using the new AWS tasks is easy to do\. Consider expanding the project and adding other AWS tasks\.