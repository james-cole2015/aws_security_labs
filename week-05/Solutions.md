Challenges: 

- create a bastion host that allows ssh access only from the publicly available host 
    -- create a security group that allows 443 & 22 from the internet to the bastion host 
        --- the security group should only allow 22 traffic from YOUR IP 
    -- create a securirty group that allows only 22 from the bastion host 
        --- make sure that this isn't specific to the instance - that we use the security group instead 

- create a NACL for the Protected Subnet and one for the Firewall subnet. 
    -- the NACL for the Protected subnet should only allow in traffic from the firewall subnet. Everything else should be denied. 
    -- the NACL for the Firewall subnet should allow in traffic from the Host IP. 


- << follow the lab set up for the AWS Network Firewall. >> 




## Challenge 01 Solutions: 
01) Navigate to VPC Console. Scroll to Security -> Security Groups. Click the orange "Create security group button on top right hand side. 
02) Choose a name for the security group ->`bastion-host-sg`. 
03) Enter a description for the security group -> `Allows SSH Access from My IP to the public facing EC2 instance` 
04) Remove the VPC that's pre-populated and make sure the select the VPC named `week-05-vpc`
05) For the inbound rules section, let's first create a rule that allows https from anywhere. Click "Add rule" 
    a) Type = HTTPS 
    b) Source = Anywhere IPv4
    c) Description = `allow https from anywhere` 
06) Let's add another rule that allows SSH access from only our IP. **NOTE** This will be different for everyone. Click the "Add rule" button. 
    a) Type = SSH 
    b) Source = My IP
    c) Description = `allows ssh access from only my ip`
07) We're not going to modify any Outbound rules, since security groups are stateful. Scroll down and click "Create security group"
08) Create another security group named `protected-sg`. Give it a description of `allow ssh access from bastion host sg`. Click "Add rule" 
09) Remove the VPC that's pre-populated and make sure the select the VPC named `week-05-vpc`
    a) Type = SSH
    b) Source = Custom
        1) in the CIDR Block text field, type in `bastion-host-sg`. The security group should pre-populate - click it and then add `allows ssh from bastion-host-sg` as a  description to the security group. 
    c) scroll down and click "Create security group" 
10) Navigate to the EC2 console -> Key pairs.
09) Create a keypair and name it `week-05-kp`.
make sure to use .pem not .ppk
10) Now we're ready to test this implementation. Let's create an EC2 instance. Go to the EC2 Console and click the orange "Launch Instance" button. . 
    a) Name the EC2 instance `Bastion Host`
    b) Choose Ubuntu 20.04. Make sure it's this AMI ID: `ami-0aa2b7722dc1b5612`
    c) Pick the `week-05-kp`
    d) Edit the network settings and make sure you're using the `week-05-vpc`. Choose the `Firewall` subnet. 
    e) make sure the "Auto-assign public IP" is enabled. 
    e) For security groups, choose "Select existing security group". A dropdown will appear, choose the `bastion-host-sg` 
    f) Click the orange "Launch Instance" button on the right. 
11) Now we'll create an EC2 instance inside the Protected Subnet. 
    a) Name the EC2 instance `Protected Instance`
    b) Choose Ubuntu 20.04. Make sure it's this AMI ID: `ami-0aa2b7722dc1b5612`
    c) Pick the `week-05-kp`
    d) Edit the network settings and make sure you're using the `week-05-vpc`. Choose the `Protected` subnet. 
    e) Make sure that "Auto-assign public IP" is disabled.
    e) For security groups, choose "Select existing security group". A dropdown will appear, choose the `protected-sg`
    f) Click the orange "Launch Instance" button on the right. 
12) Let's wait until both are in a running state and both are 2/2 for status check. 
13) Note the Public DNS name for the `bastion-host` instance. 
13) Let's test the security groups for the `Firewall` subnet first. Try to establish an ssh session with the `Bastion Host` instance. Move to the EC2 Console -> Instance. Click the checkbox next to the `Bastion Host` and then click `Connect` over on the right hand side. Follow the instructions for connecting to the EC2 instance using an SSH Client. 
14) If you've done everything correctly, you should be able to connect to the `Bastion Host`. This is because we've allowed ssh connectivity between your IP address and the EC2 instance. Exit out of this and return to the EC2 Console. 
15) Let's try to connect to the `Protected Instance`. Repeat step 12. 
16) Notice that you're unable to connect to this instance. Why is this? We set up a rule for ssh connection. However, we set it up so that only the `Bastion Host sg` would be able to connect to this host. What does this mean? This means that we have to use the Bastion Host instance to leapfrog our way into the `Protected Instance`. So let's do that. 
17) Follow step 12 again and log into the `Bastion Host`. Now, open up a terminal on your home computer. This is where you should have downloaded the `week-05-kp`. Run a `cat week-05-kp.pem`. This is the private key that allows us into the EC2 instance. Copy that key over to the `Bastion Host` instance. You should use a command similar to this: `scp -i <path-to-week-05-kp.pem> week-05-kp.pem ubuntu@<dns_name_of_bastion_host>:/home/ubuntu`. This will securely copy the key over to the `Bastion Host` instance. 
18) Now, when you move back to your `Bastion Host` instance, you should run the `ll` command in the home directory and you should see the `week-05-kp.pem` listed. Now you should be able to repeat step 13 again and try to log into the `Protected Instance` again. This time it should work.
19) Leave the instances and security groups running as we'll need them for the next lab. 

*** Challenge 2 Solutions: 
01) Navigate to VPC Console -> Security -> Network ACLs
02) Click the orange "Create network ACL"
03) Give it a name of `week-05-firewall-acl`
    - VPC = `week-05-vpc`
    - Click "Create network ACL" 
04) Repeat Step 03, but give it a name of `week-05-protected-acl`
05) Click the checkbox for the `week-05-firewall-acl` and Click the the "Actions" dropdown. Select "Edit Inbound Rules" 
06) Add the following rules: 
    a) Rule number = 100
    b) Type = SSH 
    c) Source = <your IP with a CIDR block of /16. E.g., if your IP is '17.222.133.18', enter '17.222.133.18/16'>. 
        1) This is meant to simulate a corporate environment CIDR range. You can visit: https://whatismyipaddress.com/ to get your IP address. 
    d) Allow/Deny = Allow

    a) Rule # = 120
    b) Type = HTTPS
    c) Source = "0.0.0.0/0"
    d) Allow/Deny = Allow

    a) Rule # = 130
    b) Type = All ICMP - IPv4
    c) Source = <your IP with a CIDR block of /16. E.g., if your IP is '17.222.133.18', enter '17.222.133.18/16'>. 
    d) Allow/Deny = Allow

    e) Click saves changes. 

07) Click the checkbox for the `week-05-firewall-acl` and Click the the "Actions" dropdown. Select "Edit Outbound Rules" 
08) Add the following rules: 
    a) 




Environment Clean Up: 
- Terminate all of the EC2 instances 
- Delete all of the created security groups
- Delete the `week-05-kp`