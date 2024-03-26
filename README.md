# S3-based CDN with CloudFront Deployment Guide

This guide outlines the steps to deploy an S3-based Content Delivery Network (CDN) using Amazon CloudFront, leveraging AWS CloudFormation for the setup. This solution is designed to deliver content globally with low latency and high transfer speeds, secured by CloudFront's Origin Access Control.

## Prerequisites

Before starting, ensure you have the following:

- An active AWS account.
- AWS CLI installed and configured on your machine. [AWS CLI Installation Guide](https://aws.amazon.com/cli/)
- Familiarity with AWS services, particularly S3 and CloudFront.

## Template Overview

The CloudFormation template includes the following key resources:

- **S3 Bucket**: A private bucket to store your static website content.
- **CloudFront Origin Access Control (OAC)**: Enhances security by ensuring CloudFront can securely access content in your S3 bucket.
- **CloudFront Distribution**: The CDN distribution to serve your content globally.
- **Cache Policy**: An optional parameter for specifying the cache policy ID to be used by CloudFront. A default value is provided but can be overridden if needed.


## Deployment Steps

### 1. Prepare the CloudFormation Template

Save the provided YAML configuration as `s3_cdn_cloudfront.yaml`. The template uses parameters for flexibility, allowing you to specify the S3 bucket name during stack creation.

### 2. Deploy the CloudFormation Stack

#### Using AWS CLI

1. Open a terminal.
2. Run the following command, replacing `YOUR_UNIQUE_BUCKET_NAME` with your desired S3 bucket name and `YOUR_STACK_NAME` with a name for your CloudFormation stack:

```bash
aws cloudformation create-stack --stack-name YOUR_STACK_NAME --template-body file://s3_cdn_cloudfront.yaml --parameters ParameterKey=S3BucketName,ParameterValue=YOUR_UNIQUE_BUCKET_NAME --capabilities CAPABILITY_NAMED_IAM
```
If you want to change the cache policy
```bash
aws cloudformation create-stack --stack-name YOUR_STACK_NAME --template-body file://s3_cdn_cloudfront.yaml --parameters ParameterKey=S3BucketName,ParameterValue=YOUR_UNIQUE_BUCKET_NAME ParameterKey=CachePolicyId,ParameterValue=YOUR_CACHE_POLICY_ID --capabilities CAPABILITY_NAMED_IAM
```

This command initiates the stack creation process, which may take a few minutes to complete.

#### Using AWS Management Console

1. Log in to the AWS Management Console and navigate to the CloudFormation service.
2. Click **Create stack** > **With new resources (standard)**.
3. Choose **Upload a template file**, click **Choose file**, and select the `s3_cdn_cloudfront.yaml` file.
4. Click **Next**. Enter a stack name and the `S3BucketName` parameter value when prompted.
5. Follow the on-screen instructions to review and create the stack.

## Post-Deployment Actions

### Accessing Your Content

Upload your static content to the newly created S3 bucket. Your content is now accessible through the CloudFront distribution URL, which you can find in the CloudFront management console.

### Managing the Stack

- To update your stack, such as changing the S3 bucket name, use the AWS CLI or Management Console to update the stack with new parameter values.
- Monitor your CloudFront distribution performance and costs through the AWS Management Console.

## Destroying the Stack

If you need to tear down your resources and delete the CloudFormation stack, you can do so using the AWS CLI or Management Console. This action will delete all resources created by the stack, including the S3 bucket and the CloudFront distribution.

Please note that if your S3 bucket contains objects, you may need to manually delete those objects or enable bucket versioning and setup a lifecycle policy to clear the bucket before deletion.

### Using AWS Management Console

1. Log in to the AWS Management Console and navigate to the CloudFormation service.
2. Select the stack you wish to delete.
3. Click **Delete**.
4. Confirm the deletion. AWS CloudFormation will then delete the stack and all associated resources.

### Using AWS CLI

Run the following command, replacing `YOUR_STACK_NAME` with the name of your CloudFormation stack:

```bash
aws cloudformation delete-stack --stack-name YOUR_STACK_NAME
```