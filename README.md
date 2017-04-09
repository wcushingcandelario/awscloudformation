# Base Cloudformation Templates
The templates in this directory form the base network configuration upon which all *other* templates are run.  The majority of the templates here are required prerequisites for other templates.  Be very careful when modifying these as they are the foundation for all accounts.

Dependencies can always be identified by searching a template for `ImportValue` in the `Resources` element.  To find the source of the dependency, search for the dependency name and identify the item in the `Outputs` element.

#### Eg:
##### Dependency to find:

```- 'Fn::ImportValue': !Sub '${ParentVPCStack}-RouteTablePublic'
```

To find the dependency, we search for `*RouteTablePublic` in the `Outputs` element.


## Template Structure
There is a choice of base templates which allow for some customization of the network (VPN, VPC, etc) components while still providing the same level of service for other templates.
### VPC* Templates
##### vpc-3azs.yaml
This template is the first template run when provisioning an account.  This template is a super-set of all vpc* templates.  Running the `vpc-3azs.yaml` template will create a full 3 Availability Zoned VPC complete with 3 subnets per AZ and a NAT Gateway per AZ.

## Optional Templates
The following templates are not required, but many will be needed based on the implementation.
#### site_to_site_vpn.yaml
Creates all the components needed for a Site to Site IPSEC tunnel.  Review the `Parameters` section of the template for the necessary inputs.

#### simplead_directory_service.yaml
Creates a SimpleAD Service for DNS forwarding from Route53.  `NOTE:` The Route53 domain needs to be created prior to running this template.  Additionally, based on SimpleAD requiring ingress security group rules and quriks of AWS, you must run `simplead_directory_service_security_group_updates.yaml` *after* `simplead_directory_service.yaml` in order to have proper security rule ingress from the remote network.
