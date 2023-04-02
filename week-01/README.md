## Week 01 Challenge: 
- **CHALLENGE:** Using AWS Config, identify any unrestricted SSH Access in your Security Groups within your AWS Account
- ***EVIDENCE:*** Provide screenshot of non-compliant security groups
- **CHALLENGE:** Using AWS Config: remediate (either automated or manually) the non-compliant resources
- ***EVIDENCE:*** Provide screenshot of now compliant security groups
- **CHALLENGE:** Using AWS Session Manager, log into the "Public RHEL" EC2 server, AFTER SSH access has been removed from the security group. 
- ***EVIDENCE:*** Once you gain access to the EC2 server, run the command "whoami" and take a screenshot. 

#### Basic Instructions:
1) Create a keypair and name it `my-key-pair`. This is required if you're using the CloudFormation template. 
2) Using the Infrastructure requirements below or the CloudFormation template, configure your environment. 
3) Set up AWS Config and add a rule for "unrestricted-ssh" and then run/evaluate the rule to check for non-compliant resources. 
4) Once identified, remediate the non-compliant resources (e.g., security groups). Ideally, you really should only modify the security that was created from the CloudFormation template. This can be done by setting up an AutoRemediation rule or you can manually remediate the rule yourself. 
5) After you've deleted or modified the SSH rule, Configure Session Manager within Systems Manager. 
6) Start a session and select the "PUBLIC RHEL" EC2 instance.
7) Run the "`whoami`" command
8) Clean up your environment by deleting the stack in CloudFormation. 


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
