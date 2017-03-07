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

Steps Needed for Full Environment:
1. Setup email for the domain that was registered.  This is needed in order to provision a certificate.
1. Create a key pair
