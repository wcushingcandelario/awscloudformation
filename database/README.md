# Database Creation
The templates within this directory rely on the creation of a VPC as well as the creation of the base security groups.

## RDS Database Creation
The `rds.yaml` template will create a RDS Multi-AZ-Database which DOES allow termination, but will take a snapshot on removal.

Sample Run:
```
aws cloudformation create-stack --stack-name test-rds --template-body file:////cloudformation/database/rds.yaml --parameters file:////cloudformation/vpc/parameters/ovc-development-rds.json
```
