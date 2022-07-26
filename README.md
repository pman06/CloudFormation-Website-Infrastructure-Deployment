# CloudFormation-Website-Deployment

This is a simple CloudFormation `.yaml` template files that provisions a VPC network on AWS, as well as a set of autoscaled servers needed to deploy a high avalability web applicaation.

![aws cloud infrastructure](https://github.com/pman06/CloudFormation-Website-Deployment/blob/master/Cloud-Deployment-Diagram.jpeg?raw=true "aws infrastructure image")

## Services Deployed

### neywork.yml

`network.yml` file deploys:

- VPC
- Internet Gateway
- Public Subnet 1 and 2
- Private Subnet 1 and 2
- Nat Gateway 1 and 2 (for infrastructures provisioned in each private subnet)
- Public Route Table and Routes


`servers.yml` file deploys:

- EC2 Launch template
- EAn Autoscaling Group with minimum size of 1 and max 4 instances
- Application Load Balancer + rules
- Load Balancer Target groups
- SecurityGroups for instances and the load balancer
- 2 Bastion servers deployed in the public subnet

*_Please note that the S3 bucket is pre-made and fileld with contents needed for the servers to run. This can also be auto provisioned and populated using `Ansible`, but that is beyond the scope of this project_*


## Prerequisites

* First make sure you have the [AWS CLI package](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html "AWS CLI setup guide") installed on your machine.
* Have an AWS account.
* A pre created S3 bucket with files needed to update the server pre-updated

## How to deploy the template

* Store the following parameters in the [AWS Parameter Store](https://console.aws.amazon.com/systems-manager/parameters "AWS Systems manager") as string
  - EnvironmentName - An environment name where the project is beign deployed
  - LaunchTemplateVersionNumber - A version number for the launch template
  - MyKey - A precreated SSH key on the AWS account
  - AMIToUse - Amazon Machine Image to use for the deployment
  - VpcCIDR - CIDR block for the VPC
  - PublicSubnet1CIDR - Public Subnet 1 CIDR block
  - PublicSubnet2CIDR - Public Subnet 2 CIDR block
  - PrivateSubnet1CIDR - Private Subnet 1 CIDR block
  - PrivateSubnet2CIDR - Private Subnet 2 CIDR block
 * Execute `aws cloudformation create-stack --stack-name your-stack-name --template-body file://network.yml --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM" --region=us-east-1`
 
 
 *_Please note we do not need to specify parameters as this is fetched from the parameter store during execution._*
 
 
 ## Enjoy!!!
