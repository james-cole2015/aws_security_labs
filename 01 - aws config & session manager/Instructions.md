#### Basic Instructions:

1) Create a keypair and name it `my-key-pair`. This is required if you're using the CloudFormation template. Note: Keypair is an EC2 feature.
<img width="678" alt="step1" src="https://user-images.githubusercontent.com/129975163/235181459-5c5d7416-c93c-4b14-9d63-8248a686a2ad.png">

2) Using the Infrastructure requirements below or the CloudFormation template, configure your environment. 
<img width="869" alt="step2" src="https://user-images.githubusercontent.com/129975163/235181507-9e63a009-cd17-45e1-b2dd-16fc175a5997.png">
<img width="848" alt="step2_2nd" src="https://user-images.githubusercontent.com/129975163/235181538-f461b0af-5432-4d82-8fb3-c2f5997fab33.png">
<img width="382" alt="step2_3rd" src="https://user-images.githubusercontent.com/129975163/235181550-a91e40df-5ae6-4668-be2f-1a536ce5be88.png">

3) Set up AWS Config using 1-click setup. 

4) Add a rule for "restricted-ssh" and then run/evaluate the rule to check for non-compliant resources. This may take a few minutes to complete evaluation.
<img width="914" alt="step3" src="https://user-images.githubusercontent.com/129975163/235786596-aea0d482-61f5-40de-873a-89f3909ae490.png">
<img width="863" alt="step3_2nd" src="https://user-images.githubusercontent.com/129975163/235786609-12b5278d-edeb-43b2-a7f3-4ea5e0d00a2f.png">
<img width="653" alt="step4" src="https://user-images.githubusercontent.com/129975163/235786677-f09c1254-0108-4e6c-941e-65591a64b6e6.png">
<img width="677" alt="step4_2nd" src="https://user-images.githubusercontent.com/129975163/235786684-8f4af316-37fe-42a3-9bde-250947159149.png">

5) Set up remediation. Under the action drop down, click manage remediation. Choose the remediation action appropriate for the challenge. Be sure to pick the correct target under the Resource ID parameter (e.g., the resource that needs to be remediated). Save your changes.
<img width="679" alt="step5" src="https://user-images.githubusercontent.com/129975163/235786718-cfc094d4-124d-4407-babe-f3411b5a56ee.png">
<img width="800" alt="step5_2nd" src="https://user-images.githubusercontent.com/129975163/235786734-85a48922-2cbf-4e4b-a0b0-bce6b89fa9cf.png">
<img width="797" alt="step5_3rd" src="https://user-images.githubusercontent.com/129975163/235786745-05276c95-d25f-4059-ba17-3a4489d74156.png">

6) Once identified, remediate the non-compliant resources (e.g., security groups). Ideally, you really should only modify the security that was created from the CloudFormation template. This can be done by setting up an AutoRemediation rule or you can manually remediate the rule yourself. 
<img width="682" alt="step6" src="https://user-images.githubusercontent.com/129975163/235786763-43b4072f-6d20-404b-9bdd-7c338db5a58c.png">
<img width="659" alt="step6_2nd" src="https://user-images.githubusercontent.com/129975163/235786776-9f81c466-123a-4e5d-9880-484890287fe9.png">

7) After you've deleted or modified the SSH rule, Naviage to Systems Manager. 
<img width="665" alt="step7" src="https://user-images.githubusercontent.com/129975163/235786802-c2f82142-36f0-47e9-990a-8a37f7d5be29.png">

8) Under the Node Management section, click on Session Manager. Click on "Start a Session" 

9) Choose an EC2 instance to start a session and click "Start session" 
<img width="467" alt="step9" src="https://user-images.githubusercontent.com/129975163/235786862-cf525ace-b111-4ecd-9a00-a0f305280217.png">

10) Run the "`whoami`" command
<img width="122" alt="step10" src="https://user-images.githubusercontent.com/129975163/235786874-297af2e3-8841-4b7c-b43e-37e035d628cf.png">

11) Clean up your environment by deleting the stack in CloudFormation.
