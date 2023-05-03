#### Basic Instructions
##### Challenge 01
1) After the stack has completed, upload the `mock-data` to the S3 bucket that was created
<img width="623" alt="step1" src="https://user-images.githubusercontent.com/129975163/235961567-bf8f3235-b100-4967-b9b6-264532576df8.png">
<img width="838" alt="step1_2nd" src="https://user-images.githubusercontent.com/129975163/235961651-04d54117-b1a0-40ef-9245-705c1f85b1f5.png">

2) Using the search bar, navigate to AWS Macie. Click Get Started, if you haven't enabled it previously. It will ask to create a service-linked role. You can view the role permissions, and then if you wish to continue, Click Enable Macie. 

3) Click "Create Job" and then you will have to wait a minute while Macie finds all the S3 buckets. 

4) Choose the bucket that you wish to scan - then as you progress, make sure you pick a "One Time Job"
<img width="605" alt="step4" src="https://user-images.githubusercontent.com/129975163/235961688-aafaf1c7-12c6-41d9-9f7a-32faebfc29ad.png">
<img width="536" alt="step4_2nd" src="https://user-images.githubusercontent.com/129975163/235961717-7e5c889a-8aff-43a3-abab-8ca5d59e1dc9.png">

5) Leave everything else as default and then pick a job name. Finally, hit submit when ready. 
<img width="535" alt="step5" src="https://user-images.githubusercontent.com/129975163/235961758-204277da-69f9-446d-9704-165b6f30da08.png">

6) It will take some time for the scan to finish. 

7) After the scan has completed, you can view the results by clicking "Show Results" and then "Show findings" 
<img width="647" alt="step7" src="https://user-images.githubusercontent.com/129975163/235961784-6e08d1ca-a1aa-4115-aef0-e4a64c79cb55.png">

8) Select the job you created and on the right hand side you'll see a section called "Financial Information" that lists the credit card numbers found
<img width="661" alt="step8" src="https://user-images.githubusercontent.com/129975163/235961806-879e4ea7-5fb2-4d73-a484-b1ebdc4c0508.png">

9) If you click the number of findings in that section, you'll be able to see a sample of where it was found. 
<img width="623" alt="step9" src="https://user-images.githubusercontent.com/129975163/235961823-f45aee35-a30a-445e-ae70-d29eab753279.png">

###### Clean up
1) Go into the S3 bucket that was created from CloudFormation and delete the `mock-data` file. 
##### Challenge 02
1) Log into the EC2 server and install the Unified CloudWatch Agent
2) Ensure that wget is installed by executing `which wget`. If you get a return of a directory location, it's installed. If not, make sure you install wget on the server. 
3) Change into the ec2-user `sudo su ec2-user` and make sure you're in the home directory `cd $HOME`
3) Download the Unified CloudWatch Agent -> `wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
4) Install the agent -> `sudo rpm -U ./amazon-cloudwatch-agent.rpm`
5) Run the wizard -> `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard`
6) Run through the wizard choosing all the defaults except for CollectD. 
7) After verifying the config set up, it will ask if you want to collect any log files. Choose yes and then add the location of the first log file you want to monitor. Run through the defaults until it asks if you have any additional logs to monitor. Complete this until finish adding all the logs you wish to monitor. 
8) After you've finished add the logs, it will ask you if you want to store in the Parameter store and which credentials to use. Choose all the defaults and eventually the wizard will finish and exit. 
9) Start the CloudWatch logs by running the following command -> `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linux -s`. This is going to start the agent by retrieving the config that was stored in the Parameter Store. It will create the CloudWatch Log Groups and start the monitoring. 
10) Once this complete, navigate to the public ip of the EC2 instance that was created, and just hit refresh a few times. 
11) Navigate to CloudWatch and then click on "Log Groups" once you're there you will see the `access log` and the `totally_real_logs` groups. Click into any of them to view the logs that were retrieved. 
###### Clean up
1) In the Log Groups window, go to Action -> Delete log group(s)
2) Move to CloudFormation and Delete the Stack. 
