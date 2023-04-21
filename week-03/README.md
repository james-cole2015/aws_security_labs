## Week 03 Challenge: 
This weeks challenge is to manage an EC2 instance without traversing the public internet. In previous weeks, we've enabled access to the EC2 instance without SSH access, but we managed the instance while crossing the internet. 
## Week 03 - PreReqs: 
- Create a keypair. If you still have the keypair from last week, that will be fine. However, if you've deleted it from your machine, then you will need to create a new keypair. 

## How To Execute CloudFormation Template:
[Week 03 CloudFormation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-03-cf_template.yml&stackName=week-03-stack)
1. Click the link above.
3. Select a keypair
4. Choose a Random Number for the S3 bucket. 
4. Click the acknowledgement about creating IAM resources
5. Click "Create Stack"
6. Wait for the stack to say "create complete". This will create the resources and associations listed below.

## Required Instructions: 
**Challenge 01**
- Perform the following with an EC2 Instance in a private subnet. 
  1) Create a managed node in Fleet Manager
  2) Execute a Run Command on an EC2 Instance
  3) Start a Session via Session Manager

**Challenge 02**
- Access an S3 Bucket without moving through the public internet

**Challenge 03**
- Set up and configure VPC Flow Logs


## Bonus 
- Store Run Command Logs in an S3 bucket
- Store Session Logs in CloudWatch
- Submit Terraform code, CloudFormation template, or AWS CDK code that completes everything.


## Stack Deletion 
Once you're finished with the lab, please delete the stack inside of CloudFormation. Be sure the empty the S3 bucket before you attempt to delete the stack. You must also delete the endpoints before you delete the stack. 
