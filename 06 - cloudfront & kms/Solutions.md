Challenges: 

## Challenge 01 Solutions: 
01) Navigate to the KMS Console. View the "Customer Managed Keys". These are the keys that users have created and maintain. Click on the "AWS Managed Keys". These are the keys that AWS Manages. You may or may not see keys in here, based on what AWS services you have running that require encryption. 
<img width="877" alt="step1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/48b4059a-4453-40d0-8785-14ae6552cd4d">

02) Create a CMK. Go back to "Customer Managed Keys" and then click the orange "Create key" button. For this exercise, you will choose a Symmetric key. You will also choose "Encrypt and decrypt" for Key usage. Click on the Advanced options. These are options that allow you to further customize a key. You can choose the Key material origin, e.g., how the key gets created. For this exercise, we'll leave it as KMS. But if you have a key already created that you want to import, this is where you'd configure that. The next option is the Regionality. By default, keys are limited to the region that they are created in. However, you can create a Multi-region key if needed. Let's leave this as default and just click Next. 
<img width="599" alt="step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/942a93b8-6e12-4eee-ac23-b0fcd2ec966d">

03) You'll need to give the key an alias. An alias allows you to rotate the key material, without having the update any applications. Give the key an alias of `week-06-s3-key`. 
<img width="579" alt="step3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/b2ab4692-c3f1-4815-be17-0193052529bf">

04) Next you'll need to provide a Key administrator. These are users that are allowed to delete and update the key, but not necessarily use the key. By default, the user or role who creates the key will be granted access to the key.  Click Next. 
<img width="554" alt="step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/1d1a9e1c-e4ca-4226-9e1a-968905abefe7">

05) Now you'll need to define who can use the key. Choose your user or role for this step. 
<img width="566" alt="step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/1e420081-ce18-4c9d-b93a-c65835b8f163">

06) Review the key policy and then click "Finish" 
<img width="565" alt="step6_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/459777ff-91e9-4f95-a531-393df78e922e">
<img width="578" alt="step6_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/8073ce51-77b6-4dab-915a-374f6f63031c">

07) Let's update one of the buckets to use this key. Navigate to S3 and select the bucket that starts with `week-06-bucket..`. Navigate to the Properties section and scroll down to Default encryption. By default, it uses the AWS Managed key (SSE-S3). We didn't see this on the list of keys earlier because it wasn't actively being used. Once we upload an object to the bucket we will see it listed. However, for this step, we're going to change the default encryption, so select Edit. For the encryption key type, select the AWS Key Management Service key (SSE-KMS). Then select the option that says "Choose from your own AWS KMS keys". Find the key we just created, then hit "Save changes". You'll land back on the Properties page and note that the default encryption is now using the KMS key we just created. Let's test this by uploading an object. 
<img width="659" alt="step7_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/cb20462e-b087-4770-9f5a-149099586b26">
<img width="661" alt="step7_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ab3aacb5-476f-4385-a687-ad46c874ba88">
<img width="634" alt="step7_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e857488a-a47f-414f-9582-19d74317f922">
<img width="296" alt="step7_4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/9b48106d-c5ca-4e23-bdda-fe5e0afe0b0c">
<img width="676" alt="step7_5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2016b65d-86a4-49b1-9cd2-fc2f04a18021">

08) Upload the `index02.html` file into the S3 bucket. (Note: Click on 'Raw' once opening index02.html. Download the file and save it in html format.)
<img width="651" alt="step8" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/89ee892d-b2aa-49c1-92b3-a1869cd60e25">
<img width="871" alt="step8_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/64212ba4-5651-4714-b30d-f72d6bf816dc">

09) Once it's uploaded, click into the object and then scroll down to Server-side encryption settings. Note that we're using SSE-KMS and then it's using the key we just created. You can verify this by clicking the link of the key ARN. 
<img width="840" alt="step9" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5fbcd337-9e7f-4eed-81b3-cfeb3b5b8675">
<img width="818" alt="step9_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/a479d847-494d-4367-83f3-e017c476cafa">
<img width="686" alt="step9_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/36e1cca0-3ff4-4f5a-8cb9-18498107a1ab">

10) Now, let's see how we can change the encryption type for individual objects. Let's upload `index.html`. However, before you click upload, expand the  Properties option and scroll down to Server-side encryption. For this we'll want to specify the encryption key. Then select the override bucket settings for default encryption. Now, you will choose the Amazon S3 managed key option. Once this is done, hit upload. 
<img width="648" alt="step10_1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/06dff8ae-fc29-413e-af70-81a0a4c2d15b">
<img width="632" alt="step10_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/9277d4a1-9b30-48ad-b2dd-090a2b34ad04">

11) Now repeat step 09 and verify that SSE-S3 is the key being used. Note that this is different than the default setting of using the KMS key that we created earlier. 
<img width="650" alt="step11" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/6279e643-fa50-4111-be1a-726b9161d9e6">

## Challenge 02 Solution
01) Upload the `index.html` to the `week-06-star-wars-bucket..`. Then navigate to the CloudFront Console. Click Create distribution. For the origin domain, choose the `week-06-star-wars-bucket...` from the dropdown. The name will pre-populate. For Origin access, please choose "Origin access control settings". For the Origin access control, you'll have to create a control setting. Make sure that the signing behavior is set to "Sign requests". For the "Default root object" under Settings, make sure that you enter `index.html`.  Other selects should be left as default. Click create. Leave the other settings as default. Scroll down to the bottom and click "Create distribution". (Note: If error given due to WAF, select do not need WAF protection for lab exercise).
<img width="869" alt="c2_step1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/3cb04718-b5d3-415f-86f8-98e5f451fd4f">
<img width="550" alt="c2_step1_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/fd22d9f5-f6d1-4da3-b74b-508eb89833e0">
<img width="409" alt="c2_step1_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/61550ba9-104d-4771-b394-2779ab36aaab">
<img width="557" alt="c2_step1_4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/8bd77b84-4c9a-4ba4-a9bc-1fac7685d992">

02) Now, we will need to add a bucket policy. This will ensure that the only ones allowed to access the bucket is CloudFront. You should see a blue banner that allows you to copy the policy. Click it and then move into the `week-06-star-wars-bucket..` bucket. Update the bucket policy and paste the code into the text field. Make sure the save the changes. 
<img width="887" alt="c2_step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ba56df24-8da2-4494-909c-36c7083677b1">
<img width="632" alt="c2_step2_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/966a0c48-ca0e-4274-bdcd-64226c83ee50">
<img width="646" alt="c2_step2_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f2004ed6-c64b-4f83-8564-73468ea4a439">
<img width="637" alt="c2_step2_4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/dd1bb383-25ab-40bb-8077-ee5607bc1083">

03) Move back to the CloudFront Console. Once the CloudFront distribution is deployed, we'll test it out. It may take up to 30 minutes for it to fully deploy. Make sure that the Status = Enabled and the Last modified is filled out with a date. Once that's complete, click the ID of the distribution. You'll see a "Distribution domain name". Copy it and paste it into a browser. The website stored in S3 is now available. 
<img width="865" alt="c2_step3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/eacfeb94-75ee-40c2-935b-f8502780c6fa">
<img width="878" alt="c2_step3_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/bbb25e71-76fd-4e56-b1dd-3c8455113d07">
<img width="724" alt="c2_step3_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/cf2cef51-51c9-42d0-b704-ea7da7cec09b">

04) Let's move back to the S3 bucket that hosts the index.html. In the Properties, click the Object url and paste it into a browser tab. What happens? You get an access denied. This is because the object itself is private, unless the object is accessed by the CloudFront service, and specifically, the specific distribution name that we created. This is a great method for protecting your files from malicious actors, while also serving static content to the public. 
<img width="647" alt="c2_step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/199c879c-dea1-4631-8fb4-09cf25bbc2e6">
<img width="623" alt="c2_step4_2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/359070cb-beef-4cb7-81e1-54dd53c8a236">
<img width="617" alt="c2_step4_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/620bb5f4-bfc3-495a-9fc0-3d1225201feb">

05) Some of the features that we can enable with CloudFront is SSL certificates, geographic location restriction, and we can implement WAF ACL rules as well. One common "add-on" for this service is the ability to create a CNAME record in Route53 and redirect requests from a domain name to this content. 


## Environment Clean Up: 
***
- Disable and then destroy the CloudFront distribution. 
- Empty all of the S3 buckets that we created. 
- Schedule the KMS key for deletion. 
- Delete the stack. 
