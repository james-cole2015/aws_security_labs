## *Week 01 Infrastructure Requirements:*

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
