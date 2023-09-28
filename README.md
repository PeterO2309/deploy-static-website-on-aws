# deploy-static-website-on-aws


### Project Overview
The cloud is perfect for hosting static websites that only include HTML, CSS, and JavaScript files that require no server-side processing. The whole project has two major intentions to implement:

- Hosting a static website on S3 and
- Accessing the cached website pages using CloudFront content delivery network (CDN) service. Please remember that CloudFront offers low latency and high transfer speeds during website rendering.

Note that Static website hosting requires a public bucket, whereas CloudFront can work with public and private buckets.

In this project, you will deploy a static website to AWS by performing the following steps:

1. You will create a public S3 bucket and upload the website files to your bucket.
2. You will configure the bucket for website hosting and secure it using IAM policies.
3. You will speed up content delivery using AWS’s content distribution network service, CloudFront.
4. You will access your website in a browser using the unique CloudFront endpoint.


### Prerequisites:
- AWS Account
- Student-ready starter code - Download and unzip this file via https://drive.google.com/file/d/15vQ7-utH7wBJzdAX3eDmO9ls35J5_sEQ/view. 


### Topics Covered:
- S3 bucket creation
- S3 bucket configuration
- Website distribution via CloudFront
- Access website via web browser


###Dependencies
####### Cloud Services

- Amazon Web Services S3 - Resource hosting and deployments
- AWS CloudFront - CDN for SPA
- AWS EKS - Backend API
- AWS DynamoDB - Persistent Datastore
- AWS Cognito - User Authentication

  
####### Performance Tracking and DevOps Tools:

- AWS CloudWatch - Performance and Health checks
- Sentry - Bug Reporting
    . Alternates:
    . TBD
- Google Analytics - Usage Reporting
    . Alternates:
    . Mixpanel
- Travis (CI/CD)


####### Frameworks:

- Ionic (Javascript) (Frontend)
- Node.js (Javascript) (Backend)
= Node.js (Javascript) (Backend)

## Table of Contents

- [Create S3 Bucket](#create-s3-bucket) 
- [Upload files to S3 Bucket](#upload-files-to-s3-bucket)
- [Secure Bucket via IAM](#secure-bucket-via-iam)
- [Configure S3 Bucket](#configure-s3-bucket)
- [Distribute Website via CloudFront](#distribute-website-via-cloudfront)
- [Access Website in Web Browser](#access-website-in-web-browser)


## Create S3 Bucket

1. Navigate to the “AWS Management Console” page, type “S3” in the “Find Services” box and then select “S3”.

<img width="455" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/8b30f705-8445-41ce-b3c7-e4011a470a8b">


2. The Amazon S3 dashboard displays. Click “Create bucket”.

<img width="533" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/30dccca8-98a0-4773-839a-1ce0ab222490">


3. In the **General configuration**, enter a “Bucket name” and a region of your choice. Note: Bucket names must be globally unique.

<img width="638" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/bc60a1ab-72ef-4e67-a47f-d48fdb33f33b">


4. In the **Bucket settings for Block Public Access** section, uncheck the “Block all public access”. It will enable the public access to the bucket objects via the S3 object URL.

  ** Note** - We are allowing public access to the bucket contents because we are going to host a static website in this bucket. Hosting requires the content should be publicly readable.

   <img width="461" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/80b7fa59-5b4b-43d6-9f56-47670d8e9702">


5. Click “Next” and click “Create bucket”.

6. Once the bucket is created, click on the name of the bucket to open the bucket to the contents.

   <img width="535" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/ab9615b3-1ca6-4840-b526-f5738d93df30">
   



## Upload files to S3 Bucket

1. Once the bucket is open to its contents, click the “Upload” button.

<img width="528" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/5a3d9561-42d8-4574-9eba-43a62a71f60c">

2. Click the "Add files" and “Add folder” button, and upload the Student-ready starter code (https://drive.google.com/open?id=15vQ7-utH7wBJzdAX3eDmO9ls35J5_sEQ) folder content from your local computer to the S3 bucket.


   <img width="611" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/46f189c4-5112-4f00-8c6f-41f991ca4753">


   <img width="530" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/db38026f-2f2a-456b-aedf-a902538472d3">
   

   <img width="539" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/948ea64b-977b-4ee8-8ded-f9f406158fc4">


#### Need help with uploading the files to S3?
Sometimes the local machine's network setting or firewall might block, or the browser's Adblocker may prohibit the file upload, such as buysellads.svg file. In such a case, here are the workarounds:

- **Workaround 1**
Try using Chrome browser, and turn off the Adblocker, if not already. Here are the steps to turn off the Adblocker in Chrome:

  1. At the top right, click More (three dots) >> Settings.
  2. Click Security and Privacy >> Site Settings.
  3. Click Additional Content Settings>> Ads.
  4. Turn off Block ads on sites that show intrusive or misleading ads.

- **Workaround 2**
Use CLI commands to upload the files and folders:

  1. Verify the AWS CLI configuration. If not configured already, use:
 
     ```
      aws configure list
      aws configure 
      aws configure set aws_session_token "<TOKEN>" --profile default 
     ```

  1. Upload files

     ```
      # Create a PUBLIC bucket in the S3, and verify locally as 
      aws s3api list-buckets 

      # Download and unzip the udacity-starter-website.zip 
      cd udacity-starter-website 

      # Assuming the bucket name is my-bucket-202203081 and your PWD is the "udacity-starter-website" folder 
      # Put a single file. 
      aws s3api put-object --bucket my-bucket-202203081 --key index.html --body index.html 

      # Copy over folders from local to S3 
      aws s3 cp vendor/ s3://my-bucket-202203081/vendor/ --recursive 
      aws s3 cp css/ s3://my-bucket-202203081/css/ --recursive 
      aws s3 cp img/ s3://my-bucket-202203081/img/ --recursive 
     ```



## Secure Bucket via IAM

1. Click on the “Permissions” tab.

<img width="422" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/494d6503-1d0a-4cfc-aec3-d0c9f93f3c08">


2. The “Bucket Policy” section shows it is empty. Click on the Edit button.

<img width="503" alt="image" src="https://github.com/PeterO2309/deploy-static-website-on-aws/assets/37739166/b9e7f9cb-14b0-4802-a08e-bfae75c559ab">


3. Enter the following bucket policy replacing your-website with the name of your bucket and click “Save”.

```
 {
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::your-website/*"]
    }
  ]
}
```


You will see warnings about making your bucket public, but **this step is required for static website hosting.**

> Note - If we were not learning about static website hosting, we could have made the bucket private and wouldn't have to specify any bucket access policy explicitly. In such a case, once we set up the **CloudFront distribution**, it will automatically update the current bucket access policy to access the bucket content. The CloudFront service will make this happen by using the **Origin Access Identity** user.


### Configure S3 Bucket
### Distribute Website via CloudFront
### Access Website in Web Browser



