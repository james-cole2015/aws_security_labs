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
<img width="719" alt="challenge2_step1" src="https://user-images.githubusercontent.com/129975163/236009994-cfcb515e-375f-496a-a4ff-32a120c8075f.png">
<img width="647" alt="challenge2_step1_2nd" src="https://user-images.githubusercontent.com/129975163/236010006-8de111b0-be4d-4d3e-a908-846a2c918e57.png">

2) Ensure that wget is installed by executing `which wget`. If you get a return of a directory location, it's installed. If not, make sure you install wget on the server. 
<img width="131" alt="challenge2_step2" src="https://user-images.githubusercontent.com/129975163/236010018-bbcd6894-dbdb-4929-9c5b-9b787414abf3.png">

3) Change into the ec2-user `sudo su ec2-user` and make sure you're in the home directory `cd $HOME`
<img width="263" alt="challenge2_step3" src="https://user-images.githubusercontent.com/129975163/236010037-bf45c159-0d12-4320-a4f1-ff49ca2f9261.png">

4) Download the Unified CloudWatch Agent -> `wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
<img width="896" alt="challenge2_step4" src="https://user-images.githubusercontent.com/129975163/236010049-c0c7e60a-1992-4eb0-af10-1a70ac8cf5b4.png">

5) Install the agent -> `sudo rpm -U ./amazon-cloudwatch-agent.rpm`
<img width="570" alt="challenge2_step5" src="https://user-images.githubusercontent.com/129975163/236010062-906175d6-9e33-4b1f-b9d4-c653d68be4ab.png">

6) Run the wizard -> `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard`
<img width="709" alt="challenge2_wizard1" src="https://user-images.githubusercontent.com/129975163/236017949-324e6f22-99a3-4d33-bb3f-00b4ebd515e9.png">

7) Run through the wizard choosing all the defaults except for CollectD. 
<img width="751" alt="challenge2_wizard2" src="https://user-images.githubusercontent.com/129975163/236017964-1f2e0c95-9bbf-44f3-ba00-97730e45c69c.png">

8) After verifying the config set up, it will ask if you want to collect any log files. Choose yes and then add the location of the first log file you want to monitor. Run through the defaults until it asks if you have any additional logs to monitor. Complete this until finish adding all the logs you wish to monitor. (Note: Paths are in README - 3 paths)
<img width="386" alt="challenge2_wizard3" src="https://user-images.githubusercontent.com/129975163/236018195-97692215-0489-419c-ae03-d068492fd219.png">
<img width="430" alt="challenge2_wizard4" src="https://user-images.githubusercontent.com/129975163/236018240-53cfc9df-3696-4068-91e1-727e8eb45992.png">
<img width="391" alt="challenge2_wizard5" src="https://user-images.githubusercontent.com/129975163/236018255-beb54d53-6638-4548-872f-9631040fa308.png">

9) After you've finished add the logs, it will ask you if you want to store in the Parameter store and which credentials to use. Choose all the defaults and eventually the wizard will finish and exit. 
<img width="557" alt="challenge2_wizard6" src="https://user-images.githubusercontent.com/129975163/236018280-23f91e2c-e918-4ce2-95b8-9bb267c98da0.png">

10) Start the CloudWatch logs by running the following command -> `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linux -s`. This is going to start the agent by retrieving the config that was stored in the Parameter Store. It will create the CloudWatch Log Groups and start the monitoring. 
<img width="914" alt="challenge2_step10" src="https://user-images.githubusercontent.com/129975163/236010100-c1048080-e9aa-4cc7-ba1f-3b733ea39b9f.png">

11) Once this complete, navigate to the public ip of the EC2 instance that was created, and just hit refresh a few times. 
<img width="714" alt="challenge2_step11" src="https://user-images.githubusercontent.com/129975163/236010117-6a29f447-6674-4719-9e17-2d509df57952.png">
<img width="335" alt="challenge2_step11_2nd" src="https://user-images.githubusercontent.com/129975163/236010129-7a30ca1b-a2dc-4789-b80d-32bb853c579d.png">

12) Navigate to CloudWatch and then click on "Log Groups" once you're there you will see the `access log` and the `totally_real_logs` groups. Click into any of them to view the logs that were retrieved. 
<img width="885" alt="Screenshot 2023-05-03 103059" src="https://user-images.githubusercontent.com/129975163/236018839-7e83f1a4-bf87-43da-af64-ef45f54517af.png">

###### Clean up
1) In the Log Groups window, go to Action -> Delete log group(s)
2) Move to CloudFormation and Delete the Stack. 
