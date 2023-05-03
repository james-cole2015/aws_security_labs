#### Challenge Instructions
##### Challenge 01
1) Deploy the stack 
   - Verify that the stack has completed successfully 

<img width="232" alt="stack_created_step1" src="https://user-images.githubusercontent.com/129975163/234687327-28f1533b-b133-4f7a-a5d9-db69e1108a4e.png">

2) Navigate to Fleet Manager and verify that the neither of the ec2 instances just created isn't yet managed. 

<img width="839" alt="unmanaged_instances_step2" src="https://user-images.githubusercontent.com/129975163/234687508-1f2653ba-ac41-4247-b560-57fd1f13f442.png">

3) Go to the vpc and create an endpoint by selecting "Endpoints" on the left hand menu 

4) We'll create the ssm endpoint first 
   - Name the endpoint `ssm-endpoint` 
   - Make sure that AWS Services is selected
   - In the search box, search for `ssm`. Make sure you choose the correct one, it should be named `com.amazonaws.{region}.ssm`
   - Click the radio button next to it 
   
<img width="684" alt="ssm_step4" src="https://user-images.githubusercontent.com/129975163/234688104-09c3bb68-be65-4039-a0a3-9d33be25c95b.png">

   - Select the correct vpc (the one created by the stack) in the dropdown menu
   - Using the check box, select both of the subnets available, and then select the subnet id from the dropdown menu
   - Select IPv4 for IP Address Type

<img width="610" alt="subnets_step4_2nd" src="https://user-images.githubusercontent.com/129975163/234688254-04d3f6f0-15f2-4e38-9f82-b41385b70f6a.png">

   - For Security Groups, choose the one named `endpoint-sg`
   - Leave the policy blank 
   - Click "create endpoint" and then wait for it to be in the "available status" 
<img width="714" alt="ssm_endpoint_step4_3rd" src="https://user-images.githubusercontent.com/129975163/234688373-5e70db50-000f-4632-bcc3-5fe725712d55.png">

5) Reboot the EC2 instance and wait about 5 min. 
<img width="739" alt="reboot_step5" src="https://user-images.githubusercontent.com/129975163/234689366-cb7d5f7f-631b-4604-903a-debd9753ebb7.png">

6) Navigate to Fleet Manager and confirm that the ec2 instance is now showing up as being managed. 
   *Note: it may take 5-10 minutes for this to reflect in Fleet Manager
<img width="827" alt="managed_instances_step6" src="https://user-images.githubusercontent.com/129975163/234689524-c97e627d-c861-407d-b49e-392aaf2b4e22.png">

7) Find the "Run Command" console under Systems Manager. Click the orange Run Command button. In the search box, type in `Shell` and hit enter. Select the radio button for the one named `AWS-RunShellScript`. 
<img width="139" alt="step7" src="https://user-images.githubusercontent.com/129975163/234689618-3a249a3b-6db7-443f-8da5-af2eef8df175.png">
<img width="671" alt="step7_2nd" src="https://user-images.githubusercontent.com/129975163/234689629-9ced1840-672c-4586-bf21-3a35867b2822.png">

8) Scroll down and in the Command Parameters text box, type the following `hostname`. Under target selection, click Choose Instances manually. Select the `Week-03 Ubuntu Instance` instance. 
<img width="664" alt="step8" src="https://user-images.githubusercontent.com/129975163/234689667-032bd587-256c-422d-b2d2-f00f9a8c5ad2.png">
<img width="665" alt="step8_2nd" src="https://user-images.githubusercontent.com/129975163/234689675-33156cca-0f3f-4d8b-98db-aee039cb514e.png">

9) Under output options, de-select "Enable an S3 bucket". Then scroll down to the bottom and hit "Run". You'll notice that this is "In Progress" but delayed. This is because while the SSM Endpoint is active, 
it can't communicate without the `ec2-messages` endpoint. Let's configure that.
<img width="662" alt="step9" src="https://user-images.githubusercontent.com/129975163/234689691-ed82a91d-e70c-403d-ade6-d5e3b56db263.png">
<img width="686" alt="step9_2nd" src="https://user-images.githubusercontent.com/129975163/234689704-119c4c52-634c-46ba-9b69-38b9edb4f702.png">

10) Let's cancel this command before we get the endpoint set up. Hit Cancel Command and then confirm it. 
<img width="478" alt="step10" src="https://user-images.githubusercontent.com/129975163/234689718-24c05abc-62fe-4f7a-af08-55c98456d8fe.png">
<img width="681" alt="step10_confirmation" src="https://user-images.githubusercontent.com/129975163/234689767-c381893d-02d5-4646-9df1-8c6021994aee.png">

11) Repeat step 4 but now do it for `ec2messages`.
<img width="656" alt="step11" src="https://user-images.githubusercontent.com/129975163/234689786-6ed988d6-0e6c-49f1-9a2b-6c2d7acc20c0.png">
<img width="637" alt="step11_2nd" src="https://user-images.githubusercontent.com/129975163/234689813-17853f9f-bf53-4e9d-aaa1-1041d3992fd0.png">
<img width="650" alt="step11_3rd" src="https://user-images.githubusercontent.com/129975163/234689855-72f24a36-f820-4203-a0ac-ccfd65201fcc.png">
<img width="636" alt="step11_4th" src="https://user-images.githubusercontent.com/129975163/234689875-e6bd42d3-b0da-4936-9b67-eb5c9d819385.png">
<img width="719" alt="step11_confirmation" src="https://user-images.githubusercontent.com/129975163/234689905-3f803cab-913a-4481-a182-5b6f59b3c3a7.png">

12) Repeat steps 7 through 9. The command should succeed now that we've got the `ec2_messages` enabled. 
<img width="667" alt="step12" src="https://user-images.githubusercontent.com/129975163/234689929-3a3207ef-cfe2-4d93-80d5-b1353b4fa655.png">

13) Move back to the EC2 Instances screen and attempt to connect to the Ubuntu instance. Notice that the Session Manager says it's unable to connect. Why is this? We've set up the `ec2_messages` and the `ssm endpoints`. 
    While this can allow Systems Manager to execute Run Commands and mange the instance, the Session Manager required the `ssm-messages` endpoint to facilitate connection. Let's set up that endpoint. 
<img width="728" alt="step13" src="https://user-images.githubusercontent.com/129975163/234689947-d0688b7d-1513-46d2-85b7-5e125d831157.png">
<img width="621" alt="step13_2nd" src="https://user-images.githubusercontent.com/129975163/234689973-dcf6bd50-bc30-4c92-9b0f-0660e3514e46.png">

14) Repeat step 4, but use the `ssm-messages` instead.  
<img width="630" alt="step14" src="https://user-images.githubusercontent.com/129975163/234690015-b2e3505b-ece4-42b8-bb1b-a80c9dec495c.png">
<img width="706" alt="step14_2nd" src="https://user-images.githubusercontent.com/129975163/234690040-ee0071bc-7860-4a50-a64c-c113ea88b8e7.png">

15) Reboot the EC2 instance and wait about 5 min. 

16) Attempt to connect to the EC2 instance by using Session Manager. You should be able to connect now. If it doesn't work right away, reboot the instance and wait about 5-10 minutes. If you're still not able to connect, check out the Reachability Analyzer. 
<img width="649" alt="step16_2nd_revised" src="https://user-images.githubusercontent.com/129975163/234690121-b998a238-8e7e-4dc4-947c-dc8417cb6374.png">
<img width="953" alt="step16_3rd" src="https://user-images.githubusercontent.com/129975163/234691229-9aa1ea07-f3a3-466d-a413-7c3cee40886f.png">
<img width="667" alt="step16_confirm1" src="https://user-images.githubusercontent.com/129975163/234691257-3dc6c207-7750-4cab-8a32-e315b7e577b8.png">



##### Challenge 02
1) Connect to the Amazon Linux instance and run the `aws s3 ls` command.
<img width="337" alt="challenge2_step1" src="https://user-images.githubusercontent.com/129975163/234692218-9e02106b-483a-4aab-9b3a-0d4eca1de3f1.png">
<img width="671" alt="challenge2_step1_2nd" src="https://user-images.githubusercontent.com/129975163/234692252-d28fc63c-9fc5-436e-90ee-69ce14889f08.png">

2) The command will fail. Why? 
<img width="487" alt="challenge2_step2" src="https://user-images.githubusercontent.com/129975163/234694049-94c2457d-155f-48e2-bed3-7fb90cc1652f.png">

3) Navigate to the subnet that the EC2 instance is attached to. You can view this under "Details" and "Subnet ID" 
<img width="758" alt="challenge2_step3" src="https://user-images.githubusercontent.com/129975163/234692287-24a5428a-cf6f-4041-991a-00c4853e4d48.png">

4) Select the subnet and then view the Route Table it's attached to. 
<img width="718" alt="challenge2_step4" src="https://user-images.githubusercontent.com/129975163/234692303-399ba02b-6383-4e94-8e47-61ea1d611d0c.png">

5) Note the routes. There is no route from this subnet to S3. 
<img width="695" alt="challenge2_step5" src="https://user-images.githubusercontent.com/129975163/234692318-ae8a79b5-84d9-4c09-bef8-4a302fd1cc99.png">

6) Let's create another Endpoint. Navigate to VPC -> Endpoints. 

7) Click Create Endpoint

8) Name it `s3-endpoint` 

9) Under Services, search for `s3` 

10) Choose the Gateway type of Endpoint
<img width="638" alt="challenge2_step10" src="https://user-images.githubusercontent.com/129975163/234692393-97a0f47a-a70b-43ca-a29c-2c825589c153.png">

11) Choose the VPC for this week, and this time instead of subnets, we'll be looking at Route Tables. Make sure you select both Route Tables. Leave the Policy as full access for now. Add any tags and then click Create Endpoint. 

12) Now navigate back to the Route Tables for the subnets and look at the Routes. Notice the difference? The Endpoint created a route in the Route Table that allows traffic from this VPC to the Gateway Endpoint. 
<img width="742" alt="challenge2_step12" src="https://user-images.githubusercontent.com/129975163/234692411-20d9a7aa-aca7-450f-86e9-9d9e9c12b5fd.png">
<img width="695" alt="challenge2_step12_2nd" src="https://user-images.githubusercontent.com/129975163/234692430-01f2433b-8cc9-4315-a4aa-35ec20bbee72.png">

13) Go back to the EC2 Instances, and log-in to the EC2 Instance. Run the `aws s3 ls` command again. What happens? A different error! Progress! Now we can presume that we're connected, but we're having permissions errors. How can we fix this? There's no bucket policy that needs to be evaluated because we're simply trying to list the buckets. 
<img width="920" alt="challenge2_step13" src="https://user-images.githubusercontent.com/129975163/234694076-65b9c3f7-a991-4818-bda0-1e03adf0e48e.png">

14) Go back to the EC2 Instances screen and select the EC2 Instance that we connected to. Click on "Security" and click on the IAM Role being used.
<img width="731" alt="challenge2_step14" src="https://user-images.githubusercontent.com/129975163/234692512-9c3a09be-759c-4eb4-9a0b-74a075d61d14.png">

15) Click on "Add Permissions" and choose Attach Policy. Search for `S3Read`. Click the check box and then "Add Policy". Verify that the policy was added to the Role.
<img width="883" alt="challenge2_step15" src="https://user-images.githubusercontent.com/129975163/234692526-629950f9-193f-4896-b62f-5793498aee64.png">
<img width="833" alt="challenge2_step15_2nd" src="https://user-images.githubusercontent.com/129975163/234692562-0c9c7180-0bc5-4bb9-9ede-335b072c336a.png">
<img width="643" alt="challenge2_step15_3rd" src="https://user-images.githubusercontent.com/129975163/234692574-09933dbb-f270-4136-af68-d4548b786f05.png">


16) Connect back to the EC2 Instance if you've left it already. Now try the same command: `aws s3 ls` again. If everything was set up correctly, you should get an listing of the S3 Buckets in the account. If it doesn't work immediately, give it about a minute and it should work. 
<img width="378" alt="challenge2_step16" src="https://user-images.githubusercontent.com/129975163/234692580-cf73ffed-2f15-4c10-b9dc-567855e16156.png">

##### Challenge 03
1) Navigate to the week-03-vpc and select the VPC 
<img width="708" alt="challenge3_step1" src="https://user-images.githubusercontent.com/129975163/234694502-b673e3c2-993e-460d-b3cc-9bcc64591ce2.png">

2) Select "Flow Logs" and then click "Create flow log" 

3) Choose a name like `week-03-flow-logs`

4) Under Maximum aggregation interval, choose 1 minute 

5) For the Destination log group, choose the log group named `vpc-log-group`

6) For the IAM Role, make sure you choose the `vpc-flow-logs-role` 

7) You can add some tags if you like, but it's not required. 
<img width="652" alt="challenge3_step3-7" src="https://user-images.githubusercontent.com/129975163/234694523-998e5975-0295-4c3f-b882-d3d5c12bae3b.png">

8) Click "Create flow log" and the logs will be created. 

9) You'll need to wait a few minutes, as it gathers logs every minute. However, connect to the instance using Session Manager to generate some logs 

10) Navigate to CloudWatch, on the left hand menu, Under Logs, click on Log Groups. Click on the created log group named `vpc-flow-logs`

11) Select any of the log streams and view your logs. 
<img width="908" alt="challenge3_step11" src="https://user-images.githubusercontent.com/129975163/234694589-b174fdfb-2abb-4a94-8a81-c74a3e615e1d.png">
<img width="704" alt="challenge3_step11_2nd" src="https://user-images.githubusercontent.com/129975163/234694608-71abdaa7-ca66-4654-84bc-4af481242c71.png">

##### Reachability Analyzer
If you're having issues completing the labs, please utilize the VPC Reachability Analyzer. This will test the network connectivity between two points (in this case, between the instance that was created and the endpoint we created). This is a good way of determining if you IAM permission issues or network issues. 
1) Search for Reachability Analyzer in the search bar. 
2) Click "Create and analyze path"
3) Give it a name like "ec2 instance to ssm-endpoint"
4) Under Path Source, chose the source type. In this case, it will be Instances. 
5) Under Source, choose the Instance ID of the EC2 Instance you want to check. 
6) Chose "VPC Endpoints" for Destination Source and the VPC Endpoint that you wish to check. In this case, let's choose the ssmmessages endpoint. 
7) Scroll down and click "Create and analyze path" 
8) Give it a few minutes and it will determine if there is connectivity between the two points. It will analyze any NACLs and Security Groups that will prevent or allow access. 
9) After it's complete, it will let you know if it's Reachable or Not Reachable. If it's not reachable, it will tell you why in the Path Details.
<img width="641" alt="reachability_analyzer" src="https://user-images.githubusercontent.com/129975163/234694621-b05857f7-6213-4a1f-b241-cccc2f014483.png">

##### Clean Up
1) Delete all of the endpoints that have been created. 
   - This may take a few minutes. 
2) Delete the vpc-flow-logs group.
3) Check the VPC dashboard to see if any other resources need to be deleted.
4) After the endpoints are deleted, make sure to delete the stack in CloudFormation. 
5) Double check that the ec2 instances have been terminated.

