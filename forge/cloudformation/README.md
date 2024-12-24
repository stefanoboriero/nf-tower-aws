# ComputeEnvironment config

To configure a [Seqera AWS Batch compute environment](https://docs.seqera.io/platform/latest/compute-envs/aws-batch) in its Forge flavour some AWS resources need to be provisioned beforehand in your AWS account. The CloudFormation template in [template.yaml](template.yaml) can be used to provision these resources.

## Anatomy of the template

### Created resources

The template will deploy the following resources:
- An IAM User
- An IAM Role that can be assumed by the user
- Two IAM ManagedPolicies that define the permissions for the role, to provision the Batch compute environment and to launch Pipelines
- An AccessKey / Secret pair for the user that will be used by Seqera Platform to assume the role created
- An S3 Bucket

### Accepted parameter

The following parameters are accepted to customise the names of the resources created:

- IAMUserName: will define the name of the IAM user created, defaults to `SeqeraPlatform`.
- RoleName: will define the name of the IAM role to be assumed, defaults to `SeqeraPlatformRole`.
- S3BucketName: will define the name of the S3 bucket created, defaults to `seqera-platform-data`.

### Generated Output

The stack will make available as output the following information:

- SeqeraPlatformUserAccessKeyId: the access key id to be used when configuring the Seqera Platform credentials.
- SeqeraPlatformUserSecretAccessKey: the secret access key to be used when configuring the Seqera Platform credentials.
- SeqeraPlatformRole: the role arn to be used when configuring the Seqera Platform credentials.
- SeqeraPlatformBucket: the role arn to be used when configuring the Seqera Platform credentials.

## Usage

There are 2 ways to provision resources using this template: through the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) or through the [AWS console](https://aws.amazon.com/console/)

### Usage with AWS CLI

To create the stack through the AWS CLI, download the file [template.yaml](template.yaml) on your machine, and from the same directory run:

```bash
aws cloudformation create-stack --stack-name seqera-bootstrap --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM
```

> [!NOTE]
> --capabilities CAPABILITY_NAMED_IAM is necessary to acknowledge the fact this template will create and manage IAM resources.

If you want to customise any of the parameters, you can pass them to the aws cli command following its convetion. For example to customise the IAM use name you can create the stack with the following:

```bash
aws cloudformation create-stack --stack-name seqera-bootstrap --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM --parameters ParameterKey=IAMUserName,ParameterValue=CustomIAMUserName
```

If you wish to make any modification to the resources after they are created, you can run the update command after modifying the template

```bash
aws cloudformation update-stack --stack-name seqera-bootstrap --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM
```

To delete the resources created with this stack, you can run:

```bash
aws cloudformation delete-stack --stack-name seqera-bootstrap
```
