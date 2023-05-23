## How To Execute CloudFormation Template:
[Week 06 CloudFormation Quick Create Link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://aws-security-labs.s3.amazonaws.com/week-06-cf_template.yml&stackName=week-06-stack)
1. Click the link above.
2. Click "Create Stack"
3. Wait for the stack to say "create complete". This will create the resources required for the challenge. 

## Required Instructions: 
### **<u>Challenge 01**
***
***Problem Statement:*** 
You have a client who has decided that they want to stop using the AWS Managed Keys for encryption in their S3 buckets for their most sensitive objects. Due to regulatory requirements, they have to maintain control over the creation, usage, and deletion of encryption keys. 

***Customer Requirements:***
The client has asked you to create a proof of concept solution that will allow them to use both the AWS Managed keys, as well as keys that they create in KMS. Objects in the provided buckets should use the default encryption normally, but sensitive objects should be use the KMS key that they created. 

### **<u>Challenge 02**
***
***Problem Statement:*** 
The customer hosts a static website in an S3 bucket. They've had a lot of security issues from attackers accessing the bucket objects, deleting objects and unauthorized modification of the objects. 

***Customer Requirements:***
The customer would like you to increase the security of the website and lockdown the permissions of the bucket and the objects in the bucket. The website should still be accessible to the general public. They would like you to create a solution that allows them to host the website, but prevents attackers from directly accessing the bucket. 
