#### Basic Instructions:

1) Create a keypair and name it `my-key-pair`. This is required if you're using the CloudFormation template. Note: Keypair is an EC2 feature.
<img width="678" alt="step1" src="https://user-images.githubusercontent.com/129975163/235181459-5c5d7416-c93c-4b14-9d63-8248a686a2ad.png">

2) Using the Infrastructure requirements below or the CloudFormation template, configure your environment. 
<img width="869" alt="step2" src="https://user-images.githubusercontent.com/129975163/235181507-9e63a009-cd17-45e1-b2dd-16fc175a5997.png">
<img width="848" alt="step2_2nd" src="https://user-images.githubusercontent.com/129975163/235181538-f461b0af-5432-4d82-8fb3-c2f5997fab33.png">
<img width="382" alt="step2_3rd" src="https://user-images.githubusercontent.com/129975163/235181550-a91e40df-5ae6-4668-be2f-1a536ce5be88.png">

3) Set up AWS Config using 1-click setup. 

4) Add a rule for "restricted-ssh" and then run/evaluate the rule to check for non-compliant resources. This may take a few minutes to complete evaluation.

5) Set up remediation. Under the action drop down, click manage remediation. Choose the remediation action appropriate for the challenge. Be sure to pick the correct target under the Resource ID parameter (e.g., the resource that needs to be remediated). Save your changes.

6) Once identified, remediate the non-compliant resources (e.g., security groups). Ideally, you really should only modify the security that was created from the CloudFormation template. This can be done by setting up an AutoRemediation rule or you can manually remediate the rule yourself. 

7) After you've deleted or modified the SSH rule, Naviage to Systems Manager. 

8) Under the Node Management section, click on Session Manager. Click on "Start a Session" 

9) Choose an EC2 instance to start a session and click "Start session" 

10) Run the "`whoami`" command

11) Clean up your environment by deleting the stack in CloudFormation.
