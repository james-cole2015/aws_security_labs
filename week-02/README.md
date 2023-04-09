## Week 02 Challenge: 
- CHALLENGE 01: Using AWS CloudWatch, gather application (web server or fake) metrics from the EC2 Instance
- EVIDENCE 01: Provide a screenshot of metrics from CloudWatch Log Group
- CHALLENGE 02: Using AWS Macie, perform a scan of an S3 Bucket and find any misconfigured buckets or PII within an S3 bucket. 
- EVIDENCE 02: Provide screenshot of PII captured from AWS MACIE
## Bonus Points.
- Create an SNS Topic that sends a notification to admins (e.g., yourself) when PII is found in an S3 bucket. 
- Submit Terraform code, CloudFormation template, or AWS CDK code that completes everything.
## Week 02 - PreReqs: 
- Create a keypair. If you still have the keypair from last week, that will be fine. However, if you've deleted it from your machine, then you will need to create a new keypair. 
- A random number to make the S3 bucket unique. If it's not the CloudFormation template will fail. 
- Building on last week, the security group provided **DOES NOT** provide ssh access. Either add it in yourself, or use the Session Manager to log into the EC2 instance. 
## How To Execute CloudFormation Template:
{{ LINK }} 
1. Click the link above.
2. Type a random number
3. Select a keypair
4. Click the acknowledgement about creating IAM resources
5. Click "Create Stack"
6. Wait for the stack to say "create complete". This will create the resources and associations listed below.

## Required Instructions: 
**Challenge 01**
- Once the stack is completed, upload the file named `mock-data` into the S3 bucket. 
**Challenge 02**
- Once the stack is completed, install the CloudWatch Agent. Run through the wizard, using the defaults except for the question about StatsD. Do not collect metrics for StatsD. This will cause the agent to fail as this isn't installed. 
- When it asks for logs to track, say yes. You will need to add the following three directories: 
- `/var/log/httpd/access_logs`
- `/var/log/httpd/error_log`
- `/ec2-user/home/totally_real_logs`


## Bonus 
- Create a CloudWatch Alarm based on CPU utilization. 
- Install `stress` utility on the EC2 server and execute the program. 
- Wait a few minutes (5-10), and watch to see if the CloudWatch Alarm goes off and sends a notification. 

## Stack Deletion 
Once you're finished with the lab, please delete the stack inside of CloudFormation. Otherwise, it will continue to send metrics to CloudWatch. Also be sure to go into the settings of Macie and disable Macie so it no longer scans your environment. 

## *Week 02 Basic Infrastructure Requirements:*
**VPC**
    CIDR of "/16"
**1 Subnets**
    1 Public
**Internet Gateway
NAT Gateway
Route Tables**
    1 for Public Subnet
**Elastic IP Address
Security Group**
    Allow HTTP, HTTPS from anywhere
**EC2 Instance in Public Subnet**
    Add userdata to install SSM Agent, create http server, and install flog utility 
    IAM Role attached with AmazonSSMManagedInstanceCore & CloudWatchAgentServer policy
