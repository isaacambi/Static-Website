# Static-Website

![image](cdn.jpg)
1. Go to the AWS management console and signin
2. Make sure you are in the region where you want to create your s3 bucket for this region
3. Navigate to the S3 service in the AWS management console
4. Click on Create bucket

![image](cloudfront.png)




5. provide a unique name for your bucket and select the AWS Region where you want to create the bucket

![image](unique.png)



6. Select Acls (enabled)

![image](acl.png)



7. You will see the block all public access settings. By default, all options under this setting are checked to prevent pubblic access
   1. Uncheck the "Block all public access option". This action will automatically uncheck all related actions beneath it
   2. A warning message will appear emphasizing the risks associated with making your bucket publicly acessible. Below this message you will find an acknowledgement checkbox
   4. check the acknowlegement box to confirm that you understand the consequences of enabling public access to your bucket
  
![image](block.png)

![image](acknowledge.png)

8. Click create bucket to finalize the bucket creation process

![image](click.png)

9. To allow public access, you will need to add a bucket policy explicitly granting such permissions
10. In the s3 dashboard, click on your newly created bucket's name

![image](newbucket.png)

11. Go to the Permissions tab

![image](permission.png)

12. Scroll to the bucket policy section and click "Edit"

![image](edit.png)

13. In the policy, copy and paste the following and replace YOUR_BUCKET_NAME with the name of your S3 bucket:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AddPerm",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::watchmanzik/*"
    }
  ]
}
