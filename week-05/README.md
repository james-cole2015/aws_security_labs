## Week 05 Challenge: 

## Week 05 - PreReqs: 
- Create a keypair. If you still have the keypair from last week, that will be fine. However, if you've deleted it from your machine, then you will need to create a new keypair. 

## How To Execute CloudFormation Template:
[Week 05 CloudFormation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-05-cf_template.yml&stackName=week-05-stack)
1. Click the link above.
2. Click "Create Stack"
3. Wait for the stack to say "create complete". This will create the resources required for the challenge. 

## Required Instructions: 
### **<u>Challenge 01**
***
***Problem Statement:*** 
Your client has acquired a small start up that has doesn't have a very large AWS footprint. As such they are a bit unfamiliar with security groups. You've been asked to develop and implement security group rules that align with the principle of least privilege. 

***Customer Requirements:***
Develop security group rules that allows onlys ssh access from the corporate CIDR range to the bastion host. Additionally, there is a protected instance that serves as an application server. That server should only be allowed to have ssh access from the bastion host. 

### **<u>Challenge 02**
***
***Problem Statement:*** 
After awhile, the customer has realized that admins are free to stand up instances in the VPC that don't have the security group requirements. Access controls need to be applied at the subnet level. The customer already has in place configurations that force developers to launch instances into a specific subnet. However, they don't want to force developers to use a specific subnet. Develop and implement a solution that will control access at the subnet level. 

***Customer Requirements:***
The customer wants you to use the same subnets that you worked on earlier with the security groups. They want to see a proof of concept before moving forward with the broader work in their environment. 
