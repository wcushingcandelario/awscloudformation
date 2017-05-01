# Cloudformation
## AWS Account Level Templates
The templates in this directory are intented to be run __*once*__ per AWS account.  They will provision the account with shared resources such as `AWS Config` and `CloudTrail` along with the necessary supported functions.

## Recommended Service Limit Increases
When creating a new AWS account, the following Service Limits should be increased through the AWS Support Dashboard:

|   Limit Type  |        Limit       | Suggested Value |
|:-------------:|:------------------:|:---------------:|
|      VPC      |   VPC Elastic IP   |       250       |
|      VPC      |   VPCs Per Region  |        10       |
| EC2 Instances |   Instance Limit   |  100 (per type) |
|  Elastic IPs  | New VPC Elastic IP |       250       |


http://www.tablesgenerator.com/markdown_tables#

## Account Setup
The following should be setup __*manually*__ for each new AWS account.
* Password policy
* Force MFA using `forced_mfa_iam.json` TODO: move forced_mfa_iam.json into iam CloudFormation
* Disable un-used regions
* MFA on root account
* Customize IAM user login URL (IAM -> Dashboard -> Customize URL)
* Setup consolidated billing (Request is sent from OVC Demo account to new child)
* Register a domain (if needed, automatically creates public hosted zone)
* Private Hosted Zone gets created AFTER a VPC is present
* Setup MongoDB Cloud Manager
** Including Cross Account Role for access

Steps Needed for Full Environment:
1. Setup email for the domain that was registered.  This is needed in order to provision a certificate.
1. Create a key pair

## IAM Roles
### Cross Account Roles
The following roles are created to allow `Cross Account` access to the OVC main AWS AccountId 748109554602.  These roles can be used by logging into the main OVC AWS account and assuming the roles created by the template:

* ReadOnlyCrossAccount - A read only role
* AdminCrossAccount - An administrator role
### Service Roles
The `iam.yaml` template will create the basic IAM servic and instance roles needed by ElasticBeanstalk for deployment and enhanced monitoring.  The template will also create the roles needed by RDS for enhanced monitoring.

### Steps to allow Elastic Beanstalk access to Docker Hub credentials
http://docs.aws.amazon.com/AmazonS3/latest/dev/example-walkthroughs-managing-access-example2.html
Add the following S3 bucket polciy to the `OVC Demo` account's `ovc-github-access` S3 bucket.  If a policy exists, it will need to be updated to take into account the newly created AWS account you are trying to provide access to.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "Example permissions",
         "Effect": "Allow",
         "Principal": {
            "AWS": "arn:aws:iam::CHILD_AWS_ACCOUNT_ID:root"
         },
         "Action": [
            "s3:GetObject"
         ],
         "Resource": [
            "arn:aws:s3:::ovc-github-access/dockercfg"
         ]
      }
   ]
}
```

After adding or updating the bucket policy in the `OVCDemo` AWS account, the `iam.yaml` template will automatically create the appropriate policy allowing ElasticBeanstalk permission to read the credentials file from above.  Nothing more is needed other than to run the `iam.yaml` template.

### Account configurations
The `configurations` folder contains stacks that need be created only once per account.

##### cloudtrail.yaml
This stack enables CloudTrail is all regions.

##### config.yaml
This stack enables AWS Config in the current region to store the default configuration settings.  This can be expanded on by later stacks that create AWS Config rules.  Enabling it only need be done once.
