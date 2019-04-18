## Deployment parameters

Go to https://console.aws.amazon.com/s3 and navigate to main template.

`${BUCKET_NAME} -> cfn-vfx-deadline -> templates -> deadline-stack.template`

On the Overview tab find and copy `Object URL`.

Go to https://console.aws.amazon.com/cloudformation and `Create stack`.

In specify template section, paste in Amazon S3 URL you copied earlier.

Bellow are Parameters for deploying CloudFormation template. Most of the parameters have default values, however there are some that `require input`. 

| Parameter label                      | Default           | Description                                                                                                                                                               |
|--------------------------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Availability Zones                   | `Requires input`  | The list of Availability Zones to use for the subnets in the VPC. The Quick Start uses two Availability Zones from your list and preserves the logical order you specify. |
| Production VPC CIDR                  | 10.100.0.0/16     | The CIDR block for the production VPC.                                                                                                                                    |
| Production VPC private subnet 1 CIDR | 10.100.0.0/19     | The CIDR block for the private subnet located in Availability Zone 1 of the production VPC.                                                                               |
| Production VPC private subnet 2 CIDR | 10.100.32.0/19    | The CIDR block for the private subnet located in Availability Zone 2 of the production VPC.                                                                               |
| Management VPC CIDR                  | 10.10.0.0/16      | The CIDR block for the management VPC.                                                                                                                                    |
| Management VPC public subnet 1 CIDR  | 10.10.10.0/24     | The CIDR block for the public (internet-facing) subnet located in Availability Zone 1 of the management VPC.                                                              |
| Management VPC public subnet 2 CIDR  | 10.10.20.0/24     | The CIDR block for the public (internet-facing) subnet located in Availability Zone 2 of the management VPC.                                                              |
| Management VPC private subnet 1 CIDR | 10.10.96.0/21     | The CIDR block for the private subnet located in Availability Zone 1 of the management VPC.                                                                               |
| Management VPC private subnet 2 CIDR | 10.10.112.0/21    | The CIDR block for the private subnet located in Availability Zone 2 of the management VPC.                                                                               |
| Capacity of Spot Fleet instances     | 2                 | The number of instances in the Spot Fleet.                                                                                                                                |
| Deadline Repository instance type    | m5.xlarge         | The EC2 instance type for the Deadline repository.                                                                                                                        |
| Deadline Workstation instance type   | m5.xlarge         | The EC2 instance type for the Deadline workstation.                                                                                                                       |
| Notification email address           | `Requires input`  | The email address that security event notifications should be sent to. You will receive a confirmation email at this address.                                             |
| Project name                         | MyProject         | The project name to use for tagging render farm instances.                                                                                                                |
| VFX environment                      | DEV               | The type of VFX environment to set up. You can specify DEV (development), TEST (test), or PROD (production).DEV                                                           |
| Quick Start S3 bucket name           | `Requires input`  | S3 bucket name created in deployment preparation.                                                                                                                         |
| Quick Start S3 key prefix            | cfn-vfx-deadline/ | S3 key prefix for repository assets.                                                                                                                                      |
| pDeadlineRepositorySambaPassword     | `Requires input`  | Samba password for deadlineuser account used to connect to repository.                                                                                                    |
| pDeadlineUserPassword                | `Requires input`  | Password for deadlineuser account used for RDP connection.                                                                                                                 |


Once you are happy with the parameter values and required input parameters are filled in, you can create stack.

In the next step, we will set up [AWS Client VPN](../../docs/03-SetupAWSClientVPN/README.md) to connect to Deadline
Workstation via RDP.