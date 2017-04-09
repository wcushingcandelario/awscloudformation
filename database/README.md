# Database Creation
The templates within this directory rely on the creation of a VPC as well as the creation of the base security groups.

## RDS Database Creation
The `rds.yaml` template will create a RDS Multi-AZ-Database which DOES allow termination, but will take a snapshot on removal.  A Route53 CNAME entry will be created with the name `mysql-<DBInstanceName>.domain.com` for the database.  `NOTE:` Route53 must be configured

Sample Run:
```
aws cloudformation create-stack --stack-name test-rds --template-body file:////cloudformation/database/rds.yaml --parameters file:////cloudformation/vpc/parameters/ovc-development-rds.json
```


## MongoDB Database Creation
The `mongodb.yaml` template will create an Auto Scaling Group with _*N*_ instances with the MongoDB Cloud Manager automation agent running on them.  Creating the scaling group is the first step needed to create a usable Mongo database for the clients.

`NOTE:`A Mongo Cloud Manager account is required prior to running this template.  A new Mongo Cloud Manager account should be created for *each* VPC as all Mongo agents require direct communication to be "healthy".  There is no additional cost and management overhead is minimal.

The following steps are needed to get a working Mongo database:
1. Create Mongo Cloud Manager account
1. Enable `Authentication` in Mongo Cloud Manager
  * `Security` -> `Authentication & TLS/SSL` ![Alt][2]
  * `Edit Settings`
  * Enable `Username/Password` ![Alt][1]
1. Setup `users` in Mongo Cloud Manager
  * `Security -> Users`
  * Enable `Enforce Consistent State`
  * Add a user for each application that will be connecting to the DB server.
  * Users created should have `dbOwner` permissions on their database. ![Alt][3]
1. Get Mongo Cloud Managers API key and Group ID from `Settings -> Group Settings`
1. Run the CloudFormation template
1. Wait ~5 minutes
1. *_N_* instances show up in Mongo Cloud Manager
1. In MongoDB Cloud Manager, install the `Monitoring Agent` on every node. ![Alt][6]
1. Create a new Replica Set
  * `Processes -> +Add (button)`
  * `Replica Set` ![Alt][4]
  * Set `Auth Schema Version` to `3 (2.6 Style)`
  * For a 3 node cluster, under `Member Options` select 3 under `MongoDs Per Replica Set` (Arbiter is optional, all 3 can be `Default`)
  * Set the port range to `27000 to 27020` ![Alt][5]
  * Select `Apply` and then `Deploy Changes`


[1]: screenshots/auth.png "Authentication"
[2]: screenshots/security.png "Security"
[3]: screenshots/users.png "Users"
[4]: screenshots/newreplicaset.png "New Replica Set"
[5]: screenshots/replicaoptions.png "Replica Set"
[6]: screenshots/monitoringagent.png "Monitoring Agent"
