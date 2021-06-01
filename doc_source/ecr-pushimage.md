# Amazon ECR Push task<a name="ecr-pushimage"></a>

\(Amazon Elastic Container Registry Push Image Task\)

## Synopsis<a name="synopsis"></a>

Pushes a Docker image identified by name, with optional tag, or image ID to the Amazon Elastic Container Registry \(ECR\)\.

## Description<a name="description"></a>

This task pushes a Docker image to the Elastic Container Registry\. The image to push can be identified using its image ID or by name, with optional tag suffix\. The task handles the work of appropriately tagging the image as required by ECR and also the login process to your registry prior to executing the Docker Push command\.

## Parameters<a name="parameters"></a>

You can set the following parameters for the task\. Required parameters are noted by an asterisk \(\*\)\. Other parameters are optional\.

### Display name\*<a name="display-name"></a>

The default name of the task instance, which can be modified: Push Image

### AWS Credentials<a name="aws-credentials"></a>

Specifies the AWS credentials to be used by the task in the build agent environment\.

You can specify credentials using a service endpoint \(of type AWS\) in the task configuration or you can leave unspecified\. If unspecified the task will attempt to obtain credentials from the following sources in order:

- From task variables named _AWS\.AccessKeyID_, _AWS\.SecretAccessKey_ and optionally _AWS\.SessionToken_\.
- From credentials set in environment variables in the build agent process\. When using environment variables in the build agent process you may use the standard AWS environment variables: _AWS_ACCESS_KEY_ID_, _AWS_SECRET_ACCESS_KEY_ and optionally _AWS_SESSION_TOKEN_\.
- If the build agent is running on an Amazon EC2 instance, from the instance metadata associated with the EC2 instance\. For credentials to be available from EC2 instance metadata the instance must have been started with an instance profile referencing a role granting permissions to the task to make calls to AWS on your behalf\. For more information, see [Using an IAM role to grant permissions to applications running on Amazon EC2 instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html)\.

### AWS Region<a name="aws-region"></a>

The AWS Region code \(for example, us\-east\-1, us\-west\-2\) of the Region containing the AWS resources the task will use or create\. For more information, see [Regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the _Amazon Web Services General Reference_\.

If a Region is not specified in the task configuration the task will attempt to obtain the Region to be used using the standard AWS environment variable _AWS_REGION_ in the build agent process's environment\. Tasks running in build agents hosted on Amazon EC2 instances \(Windows or Linux\) will also attempt to obtain the Region using the instance metadata associated with the EC2 instance if no Region is configured on the task or set in the environment variable\.

**Note:** The Regions listed in the picker are those known at the time this software was released\. New Regions that are not listed may still be used by entering the _region code_ of the Region \(for example, _us_west_2_\)\.

### Image Identity\*<a name="image-identity"></a>

How the image to be pushed is identified\. You can select from either the image ID or the image name\. If image name is selected a tag can also be specified\.

### Source Image Name<a name="source-image-name"></a>

The name of the image to push\. Required if _Image Identity_ is set to _Image name with optional tag_\.

### Source Image Tag<a name="source-image-tag"></a>

Optional tag that can be suffixed to the image name\. If a tag is not specified, 'latest' is assumed\.

### Source Image ID<a name="source-image-id"></a>

The ID of the image to push\. Required if _Image Identity_ is set to _Image ID_\.

### Target Repository Name\*<a name="target-repository-name"></a>

The name of the repository to which the image will be pushed\.

### Target Repository Tag<a name="target-repository-tag"></a>

Optional tag for the new image in the repository\. If not specified, ECR will assume 'latest'\.

### Create repository if it does not exist<a name="create-repository-if-it-does-not-exist"></a>

If checked, the task will check to see if the repository exists and if it does not, will attempt to create it\.

### Image Tag Output Variable<a name="image-tag-output-variable"></a>

The name of a build variable that will be created or updated with the pushed image reference\. The image tag will be of the form _aws_account_id\.dkr\.ecr\.region\.amazonaws\.com/imagename_, where **imagename** is in the format _repositoryname\[:tag\]_

## Task Permissions<a name="task-permissions"></a>

This task requires permissions to call the following AWS service APIs \(depending on selected task options, not all APIs may be used\):

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability",
        "ecr:CompleteLayerUpload",
        "ecr:DescribeImages",
        "ecr:DescribeRepositories",
        "ecr:GetDownloadUrlForLayer",
        "ecr:InitiateLayerUpload",
        "ecr:ListImages",
        "ecr:PutImage",
        "ecr:UploadLayerPart"
      ],
      "Resource": "arn:aws:ecr:${REGION}:${ACCOUNT_ID}:repository/{$REGISTRY_NAME}"
    },
    {
      "Effect": "Allow",
      "Action": "ecr:GetAuthorizationToken",
      "Resource": "*"
    }
  ]
}
```

## Syntax<a name="aws-properties-amazon-ecr-push-syntax"></a>

To declare this entity in your Azure Pipelines template, use the following syntax:

### YAML<a name="amazon-ecr-push-syntax.yaml"></a>

Push an image to ECR:

```
- task: ECRPushImage@1
  inputs:
    awsCredentials: 'awsCredentials'
    regionName: 'us-west-2'
    imageSource: 'imagename'
    sourceImageName: 'imageName'
    repositoryName: 'repositoryName'
```
