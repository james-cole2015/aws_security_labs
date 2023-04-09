## Week 02 Challenge: 
- CHALLENGE 01: Using AWS CloudWatch, gather application (web or fake) metrics from the EC2 Instance
- EVIDENCE 01: Provide a screenshot of metrics from CloudWatch Log Group
- CHALLENGE 02: Using AWS Macie, perform a scan of an S3 Bucket and find any misconfigured buckets or PII within an S3 bucket. 
## Bonus Points.
- Create an SNS Topic that sends a notification to admins (e.g., yourself) when PII is found in an S3 bucket. 
- Create a CloudWatch Alarm for any metric you'd like. Can be application based (e.g., web server) or instance based (e.g., cpu utilization) 
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
