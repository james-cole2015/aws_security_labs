#### Basic Instructions:

1) Create a keypair and name it `my-key-pair`. This is required if you're using the CloudFormation template. 
2) Using the Infrastructure requirements below or the CloudFormation template, configure your environment. 
3) Set up AWS Config using 1-click setup. 
4) Add a rule for "unrestricted-ssh" and then run/evaluate the rule to check for non-compliant resources. This may take a few minutes to complete evaluation.
5) Set up remediation. Under the action drop down, click manage remediation. Choose the remediation action appropriate for the challenge. Be sure to pick the correct target under the Resource ID parameter (e.g., the resource that needs to be remediated). Save your changes.
6) Once identified, remediate the non-compliant resources (e.g., security groups). Ideally, you really should only modify the security that was created from the CloudFormation template. This can be done by setting up an AutoRemediation rule or you can manually remediate the rule yourself. 
7) After you've deleted or modified the SSH rule, Naviage to Systems Manager. 
8) Under the Node Management section, click on Session Manager. Click on "Start a Session" 
9) Choose an EC2 instance to start a session and click "Start session" 
10) Run the "`whoami`" command
11) Clean up your environment by deleting the stack in CloudFormation.
