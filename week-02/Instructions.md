#### Basic Instructions
##### Challenge 01
1) After the stack has completed, upload the `mock-data` to the S3 bucket that was created
2) Using the search bar, navigate to AWS Macie. Click Get Started, if you haven't enabled it previously. It will ask to create a service-linked role. You can view the role permissions, and then if you wish to continue, Click Enable Macie. 
3) Click "Create Job" and then you will have to wait a minute while Macie finds all the S3 buckets. 
4) Choose the bucket that you wish to scan - then as you progress, make sure you pick a "One Time Job"
5) Leave everything else as default and then pick a job name. Finally, hit submit when ready. 
6) It will take some time for the scan to finish. 
7) After the scan has completed, you can view the results by clicking "Show Results" and then "Show findings" 
8) Select the job you created and on the right hand side you'll see a section called "Financial Information" that lists the credit card numbers found
9) If you click the number of findings in that section, you'll be able to see a sample of where it was found. 
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
