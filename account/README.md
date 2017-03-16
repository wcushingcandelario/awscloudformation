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
* Setup consolidated billing
* Register a domain (if needed, automatically creates public hosted zone)
* Private Hosted Zone gets created AFTER a VPC is present
* Setup MongoDB Cloud Manager
** Including Cross Account Role for access

Steps Needed for Full Environment:
1. Setup email for the domain that was registered.  This is needed in order to provision a certificate.
1. Create a key pair

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

TODO: Add to CloudFormation

Next, add the following policy to the ElasticBeanstalk INSTANCE role:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Example",
            "Effect": "Allow",
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
