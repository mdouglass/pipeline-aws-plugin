# Features

This plugins adds Jenkins pipeline steps to interact with the AWS API.

* Invalidating CloudFront distributions
* Creating, updating and deleting CloudFormation stacks
* Up- and downloading files to/from S3

## Upcoming features

Planned features for upcoming versions include:

* Login to ECR
* S3 sync

# Usage / Steps

## withAWS

the `withAWS` step provides authorization for the nested steps. 
You can provide region and profile information or let Jenkins 
assume a role in another or the same AWS account. 
You can mix all parameters in one `withAWS` block.

Set region information:

```
withAWS(region:'eu-west-1') {
    // do something
}
```

Use profile information from `~/.aws/config`:

```
withAWS(profile:'myProfile') {
    // do something
}
```

Assume role information (account is optional and uses current account as default):

```
withAWS(role:'admin', roleAccount:'123456789012') {
    // do something
}
```

## awsIdentity

Print current AWS identity information to the log.

```
awsIdentity()
```

## cfInvalidate

Invalidate given paths in CloudFront distribution.

```
cfInvalidate(distribution:'someDistributionId', paths:['/*'])
```

## s3Upload

Upload a file from the workspace to an S3 bucket.

```
s3Upload(file:'file.txt', bucket:'my-bucket', path:'/path/to/target/')
```

## s3Download

Download a file from S3 to the local workspace. 
Set optional parameter `force` to `true` to overwrite existing file in workspace.

```
s3Download(file:'file.txt', bucket:'my-bucket', path:'/path/to/source/file.txt', force:true)
```

## cfnUpdate

Create or update the given CloudFormation stack using the given template from the workspace.
You can specify an optional list of parameters. 
You can also specify a list of `keepParams` of parameters which will use the previous value on stack updates.
The step returns the outputs of the stack as map.

```
def outputs = cfnUpdate(stack:'my-stack', file:'template.yaml', params:['InstanceType=t2.nano'], keepParams:['Version'])
```

## cfnDelete

Remove the given stack from CloudFormation.

```
cfnDelete(stack:'my-stack')
```

## cfnDescribe

The step returns the outputs of the stack as map.

```
def outputs = cfnDescribe(stack:'my-stack')
```

# Versions

## 1.2
* add proxy support using standard environment variables
* add cfnDescribe step to fetch stack outputs

## 1.1
* fixing invalidation of CloudFront distributions
* add output of stack creation, updates and deletes
* Only fetch AWS environment once 
* make long-running steps async

## 1.0
* first release containing multiple pipeline steps
