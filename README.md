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

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AddPerm",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

This policy will allow public access to all objects in your S3 objects

Add your bucket name instead of watchmanzik

![image](bucketpolicy.png)

14. To save your bucket policy, at the bottom of the page, click save changes
    the bucket overview page will load and you will see a notification that the policy has been edited

    note: if you see an error, ensure that the bucket name has been correctly entered into the policy

15. Let us enable static website hosting, click on the properties tab

![image](properties.png)

16. Scroll down to find the static website hosting option and click on edit

![image](staticimage.png)

17. Select Enable to enable static website hosting &
    1. Choose Host a Static website. Here, you will be prompted to enter:
       1. Index document:the name of your homepage document(index.html). This is the file served when visitors access the root url of your website
       2. Error document(optional): The name of the HTML file to show when an error occurs(e.g error.html).This is not mandatory but recommended for handling HTTP errors
    2. (Optional) If you have a custom error document, fill in the error document field.
    3. Click save changes
   
![image](staticweb.png)

This is the Link to the contents that you need to upload

<a href="https://github.com/techiecoder2079/Nerflix-website">Nerflix Website Repository</a>

Go to the Objects tab & Click Upload to upload your static website files (HTML, CSS, JavaScript, images, etc.).

18. Click on Add files to add files to S3 Bucket

![image](loadup.png)

19. If you are using AWS-CLI to upload your file, then you have to launch an instance, say ubuntu, for example
   create an IAM USER > generate Access keys > use aws configure command > fill in your access key ID and secret access keys
   install git on your instance > clone the repository > git clone https://github.com/techiecoder2079/Nerflix-website.git
   cd Nerflix-website > aws s3 sync Nerflix-website.git s3://watchmanzik/

![image](initialcommand.png)


![image](s3sync.png)

![image](file.png)

![image](folder.png)


20. Access website in your web browser
    1. Go back to the Properties tab and scroll down to the Static website hosting section.
   
![image](prop.png)


   2. You’ll see a Bucket website endpoint URL. This URL is the public address of your static website.

![image](url.png)

21. Paste the endpoint into the address bar of a new browser tab.
    You will see something that looks like this


![image](avenger.png)

# AMAZON CLOUDFRONT
 Amazon CloudFront is a global Content Delivery Network (CDN) that delivers data securely 
 and efficiently. CloudFront pulls your website out to the edge of the network, reducing 
 latency when accessed from different global locations.

In this last step, you will set up an Amazon CloudFront distribution for your static site
hosted in your Amazon S3 bucket, update the bucket policy to allow access to the CloudFront 
distribution, and update permissions to block public access to the S3 bucket.

## Instructions
1.  In the AWS Management Console search bar, enter CloudFront, and click the CloudFront result under Services:

![image](cloud.png)

The Amazon CloudFront console will load.
2. To start creating a distribution, click Create a CloudFront Distribution:

![image](create.png)

3. Under Origin, in the Origin Domain text-box, enter the Amazon S3 static website hosting
    endpoint that you created earlier:

Note: If you see a warning regarding the S3 website endpoint, you can safely ignore the message
and keep moving forward

![image](origin.png)

Warning: If you don’t see the S3 bucket in the origins list, manually enter the following:
<name_of_the_bucket>.s3.amazonaws.com

4. Under Origin, in the Origin access, select Origin access control settings and click Create new OAC:

![image](originaccess.png)

5. Under Create control setting, enter the following values:

    Name: by Default it will have
    Signing behavior: Ensure Sign requests is selected

![image](oac.png)

Origin access control secures S3 origins by allowing access to only designated 
distributions. This follows AWS best practice of using IAM service principals to authenticate
with S3 origins.

6. Click create

Leave all other fields in this section as well as the Default cache behavior section as their 
default values.

7. Select Do not enable security protections under Web Application Firewall (WAF):

Note: You can safely ignore the Custom SSL Certificate error

8. Scroll down to Settings, and in the Price class selection, select Use only North America and Europe

![image](webapp.png)

9. In the Default root object field, enter index.html.

CloudFront will serve the default root object when the base distribution URL is requested

You are setting this field because Amazon CloudFront doesn’t always transparently relay 
requests to the origin. If you did not set a default root object on the distribution you would
see an AccessDenied error when you access the CloudFront distribution’s domain later in this lab step.

![image](index.png)
