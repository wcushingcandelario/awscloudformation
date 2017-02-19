# Cloudformation
## AWS Account Level Templates
The templates in this directory are intented to be run _*once*_ per AWS account.  They will provision the account with shared resources such as `AWS Config` and `CloudTrail` along with the necessary supported functions.

## Recommended Service Limit Increases
When creating a new AWS account, the following Service Limits should be increased through the AWS Support Dashboard:

|   Limit Type  |        Limit       | Suggested Value |
|:-------------:|:------------------:|:---------------:|
|      VPC      |   VPC Elastic IP   |       250       |
|      VPC      |   VPCs Per Region  |        10       |
| EC2 Instances |   Instance Limit   |  100 (per type) |
|  Elastic IPs  | New VPC Elastic IP |       250       |


http://www.tablesgenerator.com/markdown_tables#
