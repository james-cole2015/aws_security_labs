## Challenge 01 Solutions: 
***
#### Enabling Console Access 
1) Navigate to the IAM Console and select Users from the menu on the left side. Find the SarahJaneSmith user. Click on the user and then find the "Security Credentials" 
2) Click the "Enable console access" under the "Console sign-in" section. 
3) Click the "Enable" button. For the password, choose a custom password. Give the user a password. Make sure to remember it.
4) Be sure the checkbox for user must create new password at next sign-in is DISABLED. Click apply. Download the password csv if you need it. 
5) Copy the console sign-in link into a text editor of your choice. 
6) Using a different browser, or a private browser, use the link for the console sign-in. Enter the IAM user name and the password you created. The account number should be auto-filled. 
7) Login and check out the EC2 instances. You should be able to see them. 
8) Attempt to reboot one of the instances. What happens? 
9) You can leave this window open for the time being. 

### Creating a Role
1) Go back to the Admin user where you created the CloudFormation Stack. 
1) Navigate to IAM console and select "Roles" from the menu on the left side. 
2) Under Trusted Entity type, select AWS Account. Be sure that "This account" is selected below. Click next. 
3) Search for "EC2" in the search bar. Find and select checkbox the `AmazonEC2FullAccess` policy. 
4) Clear the search bar by click the X next to EC2. Search for S3. Find and select the checkbox for `AmazonS3FullAccess`. Scroll down and click next. 
2) Give the role a name called `EC2_S3_FullAccess`
7) Review the JSON policy so you understand what's happening here. This policy is allowing anyone within the AWS Account that you selected to assume this role and have the permissions associated with this role. 
8) Click Create Role and make sure it completes before moving on. Click on the Role and copy the ARN. 


### Assuming a Role
1) Move back to the window where the `SarahJaneSmith` user has logged in. 
1) In the top right hand menu, where your user name `SarahJaneSmith` is located, click the dropdown. Copy down the Account ID. 
2) Towards the bottom of the menu, click the "Switch Role" button. 
3) Click the blue Switch Role button. 
4) Enter the Account ID, the Role name (`EC2_S3_FullAccess`) and pick a display name (`EC2_S3_Admin`) and then click Switch Role. 
5) This doesn't work. Why? Well, the user has permissions for EC2 and S3 ReadOnly, but does it have permissions to use STS and assume a role? We need to modify the permissions of the SarahJaneSmith user to allow this. 
6) Keep the window open, but move back to the Admin user window. 
6) Move to IAM and the user. Under permissions, click the drop down for Add Permissions. Select "Add inline policy" 
7) Under service, search and choose "STS" 
8) Expand the "Write" selection and check the `AssumeRole` box. 
9) For Resources, choose "All resources" Click Next. 
10) Name the policy `STS-Assume-Role`. Click Create Policy. 
11) Verify that the policy is added under Permissions policies. 
12) Attempt Steps 1 through 4 again. It should work this time. 
13) Verify that the role is active by rebooting any of the EC2 instances. 

## Challenge 02 Solutions: 
***
1) Navigate to the S3 Console and select the bucket that was created for you. Click on "Properties" and copy the ARN of the bucket. 
1) Navigate to the AWS Policy Generator. https://awspolicygen.s3.amazonaws.com/policygen.html
2) For the type of policy, choose "S3 Bucket Policy" 
3) Enter the following for the Statement: 
    - Principal = "*"
    - AWS Service = Amazon S3
    - Actions = s3:GetObject
    - For the ARN you will want to copy the ARN of the bucket and then add a backslash and star. This makes sure that the objects in the bucket (rather than the bucket itself) are the targets. E.g., `arn:aws:s3:::week-04-bucketname/*`
    - Next we will add conditions. Since the customer only wants objects that are tagged with a key-value pair of `environment:aws-security-labs` we will have to make sure that this is accounted for in the bucket policy. Click the "Add Conditions" link. 
        - We will need to make sure that we're adding a "StringEquals" condition, since we're looking for an exact match of a tag. 
        - Since we are looking for an existing object tag, find and select the key that matches that condition. This should be s3:ExistingObjectTag/<key>
        - Then for value, add `aws-security-labs`
        - Hit the "Add condition" button. 
    - Now you can hit the "Add Statement" to generate the statement to the policy. 
    - We will have to do some modification, but you can hit the 'Generate Policy' to get the policy created. 
4) Copy the policy JSON and then navigate back to the bucket. Select "Permissions" 
5) Under Bucket policy, select the edit button to edit the policy. 
6) Paste the policy into the text field. 
7) Here is where we'll need to modify the policy slightly. On line 14, where the policy states `<key>`, we'll need to change this to the key that is in the customer requirements. Be mindful of capitialization, because tags are case sensitive. 
8) Scroll down and hit "Save changes"
9) Upload the `index.html` file into the bucket. Upload the `index2.html` file into the bucket. Do not add any tags. Once this is complete, in the properties section of the object you will see an "Object URL". Right click and open in a new tab or window. Repeat this for both files. 
10) You will get an error. This is because of the policy that we added to the bucket. Objects can't be public unless they meet a specific tag requirement. Let's add that tag now. Leave the window or tab open. Go back to the S3 console and go to the properties of the `index.html` object. Scroll down to the Tags section. Add the key-value pair that the customer requires. Click "Save Changes". 
11) Now, go back to window or tab that you opened for the object and refresh the tab or window. Now the object is accessible because the tag matches the policy evaluation. Do the same for `index2.html` and verify that you still don't have access to that file. 


## Challenge 03 Solutions: 
***
## Enable Console Access
1) Navigate to the IAM Console and select Users from the menu on the left side. Find the AmeliaPond user. Click on the user and then find the "Security Credentials" 
2) Click the "Enable console access" under the "Console sign-in" section. 
3) Click the "Enable" button. For the password, choose a custom password. Give the user a password. Make sure to remember it.
4) Be sure the checkbox for user must create new password at next sign-in is DISABLED. Click apply. Download the password csv if you need it. 
5) Copy the console sign-in link into a text editor of your choice. 
6) Using a different browser, or a private browser, use the link for the console sign-in. Enter the IAM user name and the password you created. The account number should be auto-filled. 
7) Login and check out the EC2 instances. You shouldn't be able to see them. 
8) You can leave this window open for the time being. 

## Creating a Dynamic Policy 
***
10) Navigate to the AWS Policy Generator. https://awspolicygen.s3.amazonaws.com/policygen.html
11) We're going to select IAM Policy and this will be for EC2 Service. For actions, find and check the actions for RebootInstance, StartInstance, StopInstance. 
12) This should be for all instances ("*"), add this into the ARN field. Next we'll add conditions. 
13) Click conditions. Select StringEquals for the condition and `ec2:ResourceTag/${TagKey}` as the key. We're going to add a placeholder of `ec2-tag-value` here as we'll need to make this statement dynamic in nature. Click Add Condition. 
14) Add another condition, but this time, use the `aws:PrincipalTag/${TagKey}` key. Use `user-value` as the value. This is just a placeholder and will be replaced. Click "Add Statement. 
15) Let's add another statement. This time you should select EC2 as the service, and then under actions, find and select the following: 
    - DescribeAvailabilityZones
    - DescribeInstanceStatus
    - DescribeInstanceTypes
    - DescribeInstances
16) Use "*" in the ARN field and then click Add Statement. Review the statements, and if you're done, click "Generate Policy" 
17) Copy this into a text editor and then navigate to the IAM Console and create a new policy. Use the JSON Editor and replace the pre-generated text with the JSON policy you just generated. 
18) The problem statement that we needed 1 policy that was applied to all users. This policy had to make sure that users could only interact with the EC2 instances that had the same tag key-value pair as the users. E.g., Only users that have a tag of `Department:Production` should be able to manage the EC2 instances that have a tag of `Department:Production`. They should not be allowed to manage EC2 instances with a tag of `Department:Development` or really anything that wasn't `Department:Production`. So, to accomplish this, we need to use variables instead of hard-coding values. First, like in the second challenge, we're going to replace `TagKey` with the tag key that we want to consolidate on. So in the line that says `"ec2:ResourceTag/${TagKey}"`, replace `${TagKey}` with `Department`. 
19)  Now this is where the fun begins. Now we need to tell the policy to evaluate the value of the `Department` key for the user attempting to access the resource. For this, we're going to have to add another variable, but this time, in the "Value" portion of the statement. Let's modify the `aws:PrincipalTag/${KeyTag}` to reflect the `Department` value that we're looking for in a user. So like we did for the resource tag, replace `${TagKey}` with `Department`. Now, copy that `"aws:PrincipalTag/Department"` and replace `user-value` in the resource value in that line. So now the `ec2:Resource` line should look like this: `"ec2:ResourceTag/Department": "aws:PrincipalTag/Department"`. Let's add some brackets and dollar sign to the value portion so that the policy knows it's a variable. The final version should look like this: `"ec2:ResourceTag/Department": "${aws:PrincipalTag/Department}"`. Be sure to delete the line that starts with `aws:PrincipalTag`. If you leave this in here, the policy will not work. Click next and give the policy a name of `Dynamic_EC2_Policy`. Click Create. 
20) Back in the IAM Console, go to Users -> AmeliaPond. Let's modify this users permissions so that this policy is added. Click Add Permissions -> Add Permissions. Attach a policy directly. Search for the `Dynamic_EC2_Policy` that we just created. Click Next and then Add permissions. 

## Verifying Access 
*** 

21) Log back into the console as the `AmeliaPond` user. Navigate to EC2 and you should be able to see all of the instances. Attempt to restart the Production instance. This should work now. Attempt to restart any of the other instances. It shouldn't work because the other instances have different tags than the `AmeliaPond` user. Don't close the window just yet. 
22) Let's go back to the Admin window. Go to the IAM console --> Users. Click the `AmeliaPond`user. Go to Tags. 
23) Click "Manage Tags" Change the value of Department from 'Production' -> 'Application'. Save changes. 
24 Go back to the AmeliaPond window and try to restart the Application instance. This should work, but now you won't be able to restart the Production instance. 

## Environment Clean Up 
- Before you delete the stack, perform the following: 
- Make sure that the policies are removed from the `AmeliaPond` user. Also make sure that you disable console access. 
- Make sure you remove the `sts-assume-role` policy from the `SarahJaneSmith` user. Also make sure that you disable console access. 
- Delete any roles or policies that you have created. 
- Remove the `index.html` from the S3 bucket. 
- Delete the stack