## Week 04 Challenge: 

## Week 04 - PreReqs: 
- Create a keypair. If you still have the keypair from last week, that will be fine. However, if you've deleted it from your machine, then you will need to create a new keypair. 

## How To Execute CloudFormation Template:
[Week 04 CloudFormation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-04-cf_template.yml&stackName=week-04-stack)
1. Click the link above.
3. Select a keypair
4. Choose a Random Number for the S3 bucket. 
4. Click the acknowledgement about creating IAM resources
5. Click "Create Stack"
6. Wait for the stack to say "create complete". This will create the resources and associations listed below.

## Required Instructions: 
### **<u>Challenge 01**
***
***Problem Statement:*** The customer has an organizational requirement that all AWS IAM users shall not have privileges higher than **ReadOnly** for their resources (other than the root user). However, the organization does require that their engineers perform administrative actions on EC2 and S3. 

***Customer Requirements:***
Create a solution that will allow IAM users to have administrative permissions to EC2 and S3. You may not modify the permissions for EC2 and S3 within the identity policy attached to the user. You have been provided the "Sarah Jane Smith" IAM user to enable a proof of concept. 

### **<u>Challenge 02**
***
***Problem Statement:*** Customer is developing a simulated lab environment. They host a CloudFormation templates that will allow users to quickly deploy infrastructure to utlize. They have made sure that these templates are tagged with a key-value pair of `environment:aws-security-labs`. They want these objects to be public, but other objects in the bucket should not be public. 

***Customer Requirements:***
Develop an S3 bucket policy that will allow the customer to host CloudFormation Quick Create links for templates stored in S3. The other objects should not be publicly accessible.

### **<u>Challenge 03**
***
***Problem Statement:*** Customer has 4 groups that have EC2 instances in AWS. Production, Development, Testing, Application. The Production group should only be allowed to have access to the Production resources. They should not have access to the Development, Testing, or Application resources. They MAY be allowed to list or see the resources, but not have access to interact with them.  

***Customer Requirements:***
Develop a policy that will allow users to manage thier own resources in EC2, but will not allow them to have access to the other instances. The customer has a small IAM department and is trying to maintain the number of IAM policies being used. Be sure to only develop ONE policy that will be applied to all users. 

