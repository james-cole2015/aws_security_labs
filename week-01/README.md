## Week 01 Challenge: 
- **CHALLENGE:** Using AWS Config, identify any unrestricted SSH Access in your Security Groups within your AWS Account
- ***EVIDENCE:*** Provide screenshot of non-compliant security groups
- **CHALLENGE:** Using AWS Config: remediate (either automated or manually) the non-compliant resources
- ***EVIDENCE:*** Provide screenshot of now compliant security groups
- **CHALLENGE:** Using AWS Session Manager, log into the "Public RHEL" EC2 server, AFTER SSH access has been removed from the security group. 
- ***EVIDENCE:*** Once you gain access to the EC2 server, run the command "whoami" and take a screenshot. 

## Week 01 - PreReqs: 
- Create a keypair and name it "`my-key-pair`" 
  - The CloudFormation template has been hard-coded to expect this keypair. The Stack will fail if this keypair isn't present in your environment. 
    
## How To Execute CloudFormation Template:
[Cloud Formation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-01-template.yml&stackName=week-01-stack)
    
1) Click the link above. 
2) Click the acknowledgement about creating IAM resources
3) Click "Create Stack" 
4) Wait for the stack to complete. This will create the resources and associations listed below. 
    

#### Basic Instructions:
1) Create a keypair and name it `my-key-pair`. This is required if you're using the CloudFormation template. 
2) Using the Infrastructure requirements below or the CloudFormation template, configure your environment. 
3) Set up AWS Config using 1-click setup. 
4) Add a rule for "unrestricted-ssh" and then run/evaluate the rule to check for non-compliant resources. This may take a few minutes to complete evaluation.
5) Set up remediation. Under the action drop down, click manage remediation. Choose the remediation action appropriate for the challenge. Be sure to pick the correct target under the Resource ID parameter (e.g., the resource that needs to be remediated). Save your changes. Syst
6) Once identified, remediate the non-compliant resources (e.g., security groups). Ideally, you really should only modify the security that was created from the CloudFormation template. This can be done by setting up an AutoRemediation rule or you can manually remediate the rule yourself. 
7) After you've deleted or modified the SSH rule, Naviage to Systems Manager. 
8) Under the Node Management section, click on Session Manager. Click on "Start a Session" 
9) Choose an EC2 instance to start a session and click "Start session" 
10) Run the "`whoami`" command
11) Clean up your environment by deleting the stack in CloudFormation. hide


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
