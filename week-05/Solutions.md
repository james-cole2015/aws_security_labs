Challenges: 

- create a bastion host that allows ssh access only from the publicly available host 
    -- create a security group that allows 443 & 22 from the internet to the bastion host 
        --- the security group should only allow 22 traffic from YOUR IP 
    -- create a securirty group that allows only 22 from the bastion host 
        --- make sure that this isn't specific to the instance - that we use the security group instead 

- create a NACL for the Protected Subnet and one for the Firewall subnet. 
    -- the NACL for the Protected subnet should only allow in traffic from the firewall subnet. Everything else should be denied. 
    -- the NACL for the Firewall subnet should allow in traffic from the Host IP. 




## Challenge 01 Solutions: 
01) Navigate to VPC Console. Scroll to Security -> Security Groups. Click the orange "Create security group button on top right hand side. 
<img width="890" alt="step1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/c61ea93c-7e54-441e-9043-bc88feb7bcb5">

02) Choose a name for the security group ->`bastion-host-sg`. 

03) Enter a description for the security group -> `Allows SSH Access from My IP to the public facing EC2 instance` 

04) Remove the VPC that's pre-populated and make sure the select the VPC named `week-05-vpc`
<img width="862" alt="step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/4e8c952a-f269-4093-b293-073cac933417">

05) For the inbound rules section, let's first create a rule that allows https from anywhere. Click "Add rule" 
    a) Type = HTTPS 
    b) Source = Anywhere IPv4
    c) Description = `allow https from anywhere` 
<img width="850" alt="step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/9fd1c2ca-a7e4-4c09-ba23-b0545cd075af">

06) Let's add another rule that allows SSH access from only our IP. **NOTE** This will be different for everyone. Click the "Add rule" button. 
    a) Type = SSH 
    b) Source = My IP
    c) Description = `allows ssh access from only my ip`
<img width="857" alt="step6" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ee2c2013-48f4-4265-8ddc-6e9ac4c7c06c">

07) We're not going to modify any Outbound rules, since security groups are stateful. Scroll down and click "Create security group"

08) Create another security group named `protected-sg`. Give it a description of `allow ssh access from bastion host sg`.

09) Remove the VPC that's pre-populated and make sure the select the VPC named `week-05-vpc.` 
<img width="859" alt="step9" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/33254fe0-e7f3-44ff-9b78-d2497310e3ad">

10) For the inbound rules section, click "Add rule." 
    a) Type = SSH
    b) Source = Custom
        - In the CIDR Block text field, type in `bastion-host-sg`. The security group should pre-populate - click it and then add `allows ssh from bastion-host-sg` as a  description to the security group. 
    c) scroll down and click "Create security group" 
<img width="860" alt="step10" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5794badb-b9f1-49b0-bbd6-17200606bebb">

11) Navigate to the EC2 console -> Key pairs.

12) Create a keypair and name it `week-05-kp`.
make sure to use .pem not .ppk
<img width="583" alt="step12" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2023116d-2b88-4c79-b8e5-7950d86d478f">

13) Now we're ready to test this implementation. Let's create an EC2 instance. Go to the EC2 Console and click the orange "Launch Instance" button. 
    -  Name the EC2 instance `Bastion Host`
    -  Choose Ubuntu 20.04. Make sure it's this AMI ID: `ami-0aa2b7722dc1b5612`
    - Pick the `week-05-kp`
    - Edit the network settings and make sure you're using the `week-05-vpc`. Choose the `Firewall` subnet. 
    - Make sure the "Auto-assign public IP" is enabled. 
    - For security groups, choose "Select existing security group". A dropdown will appear, choose the `bastion-host-sg` 
    - Click the orange "Launch Instance" button on the right. 
<img width="554" alt="step13" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/3af3acef-6819-45a8-bfa8-85d152a3f313">
<img width="515" alt="step13_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/b138e076-cf88-4e6a-b396-be5437aedba1">
<img width="531" alt="step13_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/0ca3ed32-3a47-4f00-94e7-972280027e27">
<img width="550" alt="step13_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ceab90e0-cc36-402b-a129-5dca34bd2ff8">
<img width="538" alt="step13_4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f8a20173-0d7a-4ee3-9abb-0e04645d2aeb">
<img width="830" alt="step13_5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/a4082eb0-471d-4f99-a9a6-c38997245d4d">

14) Now we'll create an EC2 instance inside the Protected Subnet. 
    - Name the EC2 instance `Protected Instance`
    - Choose Ubuntu 20.04. Make sure it's this AMI ID: `ami-0aa2b7722dc1b5612`
    - Pick the `week-05-kp`
    - Edit the network settings and make sure you're using the `week-05-vpc`. Choose the `Protected` subnet. 
    - Make sure that "Auto-assign public IP" is disabled.
    - For security groups, choose "Select existing security group". A dropdown will appear, choose the `protected-sg`
    - Click the orange "Launch Instance" button on the right. 
<img width="525" alt="step14" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/74c4ceeb-506f-464b-a28a-2dfb77a1eda5">
<img width="527" alt="step14_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/fbc596a9-43e2-4f60-ad2b-03b2039c6ec2">
<img width="539" alt="step14_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/19ffaf01-7dc9-465c-bdca-028aa4f63cb3">

15) Let's wait until both are in a running state and both are 2/2 for status check. 
<img width="586" alt="step15" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/1d3e2a11-ba76-47e0-a9c1-fa911a813bf3">

16) Note the Public DNS name for the `bastion-host` instance. 
<img width="737" alt="step16" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f36d14a1-9cce-473d-89d2-3ea23bfbe70c">

17) Let's test the security groups for the `Firewall` subnet first. Try to establish an ssh session with the `Bastion Host` instance. Move to the EC2 Console -> Instance. Click the checkbox next to the `Bastion Host` and then click `Connect` over on the right hand side. Follow the instructions for connecting to the EC2 instance using an SSH Client. Note: In the example below, the command prompt is used. Also, locate where your week-05-kp.pem is stored.
<img width="749" alt="step17" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/14eb81c3-889f-405f-a5d5-5437df834155">
<img width="573" alt="step17_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/941403f2-4466-417e-9a8e-7a3f65159ca9">

18) If you've done everything correctly, you should be able to connect to the `Bastion Host`. This is because we've allowed ssh connectivity between your IP address and the EC2 instance. Exit out of this and return to the EC2 Console. 
<img width="536" alt="step18_0" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e1912d86-bc33-4b5f-a854-21f0525bd058">
<img width="862" alt="step18" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/74ba01c0-590e-4afa-8d48-076ba0b80867">

19) Let's try to connect to the `Protected Instance`. Repeat step 12. 
<img width="756" alt="step19" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/21e9fbd7-6942-44f3-831e-661d87d966f6">

20) Notice that you're unable to connect to this instance. Why is this? We set up a rule for ssh connection. However, we set it up so that only the `Bastion Host sg` would be able to connect to this host. What does this mean? This means that we have to use the Bastion Host instance to leapfrog our way into the `Protected Instance`. So let's do that. 

21) Follow step 12 again and log into the `Bastion Host`. Now, open up a terminal on your home computer. This is where you should have downloaded the `week-05-kp`. Run a `cat week-05-kp.pem`. This is the private key that allows us into the EC2 instance. Copy that key over to the `Bastion Host` instance. You should use a command similar to this: `scp -i <path-to-week-05-kp.pem> week-05-kp.pem ubuntu@<dns_name_of_bastion_host>:/home/ubuntu`. This will securely copy the key over to the `Bastion Host` instance. 
<img width="584" alt="step21_revised" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ff50d801-2993-47cb-8ca6-cc90d26eb141">
<img width="866" alt="step21_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/752dc963-9e46-4caf-958a-bd887a331c18">

22) Now, when you move back to your `Bastion Host` instance, you should run the `ll` command in the home directory and you should see the `week-05-kp.pem` listed. Now you should be able to repeat step 13 again and try to log into the `Protected Instance` again. This time it should work.</br>
<img width="454" alt="step22_0" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/817a0539-4779-4b36-9510-637fffec4e13"></br>
<img width="542" alt="step22" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e69272e2-b85d-4fd7-a0bf-09e52cedea6d">

23) Leave the instances and security groups running as we'll need them for the next lab. 

## Challenge 2 Solutions: 
01) Navigate to VPC Console -> Security -> Network ACLs

02) Click the orange "Create network ACL"
<img width="899" alt="c2_step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/0dd4bc1f-efac-4d18-b45b-d4ffcfc4ce0a">

03) Give it a name of `week-05-firewall-acl`
    - VPC = `week-05-vpc`
    - Click "Create network ACL" 
<img width="575" alt="c2_step3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5808a0be-8637-433b-9bdd-2b2d01abf871">

04) Repeat Step 03, but give it a name of `week-05-protected-acl`
<img width="587" alt="c2_step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/7fcd01ef-aed7-4aa2-b029-4e1910f094b0">

05) Let's make sure we associate this with the subnet so that it's attached. Select the `week-05-firewall-acl` and click the Actions dropdown and hit "edit subnet associations". Click the checkbox for the "Firewall Subnet" and then hit Save changes. 
<img width="721" alt="c2_step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/01404173-0112-41e9-b0c1-2b5e3cfdaaf6">
<img width="880" alt="c2_step5_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/323fcccd-d781-47ab-a072-0420880a115e">

06) Try to SSH into the `Bastion Host`. Does it work? It fails because we've attached a NACL to the subnet, but the only rule in the NACL is to deny all traffic. Nothing gets in, nothing gets out. Let's create some rules to allow connectivity. 
<img width="866" alt="c2_step6" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/4ec843bd-8aca-454a-9e4d-f9f2b30d39cb">

07) Click the checkbox for the `week-05-firewall-acl` and Click the the "Actions" dropdown. Select "Edit Inbound Rules" 
<img width="731" alt="c2_step7" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e586e979-db5e-4b9f-ada0-0231646cde69">

08) Add the following rules: 

    - Rule = 80, Type = HTTP, Source = 0.0.0.0/0, Allow
    - Rule = 100, Type = SSH, Source = <your IP/16>, Allow --> See note at bottom. 
    - Rule = 120, Type = HTTPS, Source = 0.0.0.0/0, Allow 
    - Rule = 140, Type = Custom TCP, Port Range = 32768-65535, Allow

    - Click saves changes. 
<img width="854" alt="c2_step8" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2ee451f3-0abf-4ba7-a812-06ba57557005">

09) Ok, now that we've set up some rules, let's try to SSH into the EC2 instance again. Ok, it still fails. Why? We've set up some rules, but we're still not getting connectivity. Well, the defining feature about NACLs is that they are stateless. This means that they don't care how traffic got in or out of the network, it needs a specific rule that allows it in or out. Unlike Security groups that are Stateful and will allow traffic out if the traffic is allowed in, NACLs are much more specific. To this end, we'll need to set up outbound rules that permit communication between the host and the ec2 instance. 

10) Click the checkbox for the `week-05-firewall-acl` and Click the the "Actions" dropdown. Select "Edit Outbound Rules" 
<img width="722" alt="c2_step10" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/7c7230e1-984d-44b5-86b1-e385de2656e9">

11) Add the following rules: 
    - Rule = 100, Type = HTTP, Source = 0.0.0.0/0, Allow
    - Rule = 120, Type = HTTPS, Source = 0.0.0.0/0, Allow
    - Rule = 130, Type = SSH, Source = 0.0.0.0/0, Allow
    - Rule = 140, Type = Custom TCP, Port Range = 1024-65535, Source = 0.0.0.0/0, Allow
    - Click Save changes. 
<img width="861" alt="c2_step11" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/fd68770b-5164-4260-9813-6dff54d4c204">

12) Now we need to make sure we do the same thing for the `week-05-protected-acl`. Let's add the following inbound rules: 
    - Rule = 100, Type = HTTPS, Source = 0.0.0.0/0, Allow
    - Rule = 120, Type = SSH, Source = < CIDR of `Firewall subnet` >, Allow  (Note: You can get the CIDR of your subnet in the VPC console - shown below).
    - Rule = 130, Type = HTTP, Source = 0.0.0.0/0, Allow
    - Rule = 150, Type = Custom TCP, Port Range = 1024-65535, Source = 0.0.0.0/0, Allow 
<img width="722" alt="c2_step12" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5a6690c0-4fd9-4db2-8399-4b95a482b66a">
<img width="849" alt="c2_step12_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2461dbfb-b0f4-4161-b1df-6d52a4bd415d">

13) Add some outbound rules to `week-05-protected-acl`: 
    - Rule = 100, Type = HTTP, Source = 0.0.0.0/0, Allow
    - Rule = 120, Type = HTTPS, Source = 0.0.0.0/0, Allow
    - Rule = 130, Type = SSH, Source = 0.0.0.0/0, Allow
    - Rule = 140, Type = Custom TCP, Port Range = 1024-65535, Source = < CIDR of `Firewall subnet` >, Allow
<img width="858" alt="c2_step13" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/d8b8550f-cce1-4a46-b75a-1ab6dc3d703b">
<img width="862" alt="c2_step13_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/31242b58-f5b9-4dd6-bfe8-e71b84c135a5">

<< *For this exercise, we need to simulate a corporate IP, since we don't really have a corporate IP that we can play around with for this challenge. So, you can take your IP (google "what's my IP") and then remove the last two octets. So if your IP is 63.36.236.125, then you will add 63.36.0.0/16 as your "corporate CIDR". This is not exactly security best practice because this will allow a vast amount of IPv4 into the EC2 instance, but this is just meant to simulate a corporate environment in which only a specific set of IP addresses will be allowed into an ec2 instance* >> 

## Environment Clean Up: 
***
- Terminate all of the EC2 instances 
- Delete all of the created security groups
- Delete all of the NACLs
- Delete the `week-05-kp`
- Delete the week05-stack in CloudFormation
