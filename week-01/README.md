## Week 01 Challenge: 
- **CHALLENGE 01:** Using AWS Config, identify any unrestricted SSH access in your Security Groups within your AWS Account
- ***EVIDENCE:*** Provide screenshot of non-compliant security groups
- **CHALLENGE 02:** Using AWS Config: remediate (either automated or manually) the non-compliant resources
- ***EVIDENCE:*** Provide screenshot of now compliant security groups
- **CHALLENGE 03:** Using AWS Session Manager, log into the "Public RHEL" EC2 server, AFTER SSH access has been removed from the security group. 
- ***EVIDENCE:*** Once you gain access to the EC2 server, run the command "whoami" and take a screenshot. 

## Bonus Points
- Create an automation for non-compliant resources that sends a notification to administrators when a resource is non-compliant. 
- Submit Terraform code, CloudFormation template, or AWS CDK code that completes everything. 

## Week 01 - PreReqs: 
- Create a keypair and name it "`my-key-pair`" 
  - The CloudFormation template has been hard-coded to expect this keypair. The Stack will fail if this keypair isn't present in your environment. 
    
## How To Execute CloudFormation Template:
[Cloud Formation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-01-template.yml&stackName=week-01-stack)
    
1) Click the link above. 
2) Click the acknowledgement about creating IAM resources
3) Click "Create Stack" 
4) Wait for the stack to say "create complete". This will create the resources and associations listed below. 

There are two options for this challenge: 
1) Use this as a challenge and complete without instructions. 
2) For those newer to AWS, you can use the Instructions provided in the repo. 


## *Week 01 Basic Infrastructure Requirements:*

#### **VPC**

* CIDR of "/16"

#### 2 Subnets

* 1 Public
* 1 Private

#### Internet Gateway

#### NAT Gateway

#### Route Tables

* 1 for Public Subnet
* 1 for Private Subnet

#### Elastic IP Address

#### Security Group

* Allow SSH, HTTP, HTTPS from anywhere

#### EC2 Instance in Public Subnet

* Add userdata to install SSM Agent
* IAM Role attached with AmazonSSMManagedInstanceCore policy

#### EC2 Instance in Private Subnet

* Add userdata to install SSM Agent
* IAM Role attached with AmazonSSMManagedInstanceCore policy
#### IAM Role
* AmazonSSMManagedInstanceCore
