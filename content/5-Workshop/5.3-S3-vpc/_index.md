---
title : "Deploy Cloud One File Storage Security"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

# Deployment

## S3 Bucket Creation

[CLICK HERE](https://aws.amazon.com/video/watch/5c76e13b7fe/) - If you don't have a S3 Bucket yet and you need to create one for this lab!

---

## Cloud One - File Storage Security Deployment

The best way to learn is to get our hands dirty, so let’s deploy Cloud One - File Storage Security to our AWS Account. First of all, make sure that all the requirements are meet, then:

1. Login to [Cloud One platform](https://cloudone.trendmicro.com/), it will request the login information for the Cloud One, in case you don’t have it, check the requirements page that has all the details that you will need to create your account.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/6cfeb834-fd01-445a-b103-64adf757cb2c" />

2. Select the **File Storage Security** tile.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/94319145-f628-4239-8424-0cb9337cc07e" />

3. Click on **Stack management** in your left side.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/7e9bfbb4-7ca2-4d33-aa98-a31bc1c28d05" />

4. Click on **Deploy**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/796f660d-1b35-43da-90cf-b61123fcef59" />

5. Select **Scanner Stack and Storage Stack**, to deploy the full solution to your AWS account.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3d4c17e9-be1a-472d-b860-9a0be0a239c7" />

6. Select the AWS region that best suit for you (AWS regions supported) and click on **Launch Stack**, this will redirect you to your AWS account in the region that you choose to deploy the stack. Make sure that you’re logged in and has the correct permissions, you can check the details of the permissions required in the Requirements section.
   
   > **Note:** Try to make sure the Cloud One console tab is in the same Window from your browser this way the Launch Stack will automatically use the AWS session that you have open already.
   
   *You can validate the Cloud Formation Template by clicking in **Review Stack**, to make it easier, you can also verify the Cloud Formation Template by clicking the buttons below:* [All In One Template](https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-All-In-One.template)
   [Scanner Stack](https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-Scanner-Stack.template)
   [Storage ](https://github1s.com/trendmicro/cloudone-filestorage-cloudformation-templates/blob/master/aws/FSS-Scanner-Stack.template)
<img width="800" alt="image" src="https://github.com/user-attachments/assets/c5704cdf-c717-4300-b45e-3bc24644f646" />

7. In the CloudFormation page the only required parameter here is the name of bucket that you choose to be scanned, just add it in the configuration (S3BucketToScan). In our case please use the name from the S3 bucket that you have created early in this workshop or use one existing S3 bucket that you prefer.

It also supports differents parameters to customize your installation, like Resource prefixes and optional KMS integration, for more details about these configurations check our [Documentation](https://cloudone.trendmicro.com/docs/file-storage-security/gs-deploy-all-in-one-stack/).
<img width="800" alt="image" src="https://github.com/user-attachments/assets/818717f0-6e6e-47e0-ad11-88206d2beb2d" />

After adding the bucket name you will need to acknowledge and click on "Create Stack"

8. In the CloudFormation page the only required parameter here is the name of bucket that you choose to be scanned, just add it in the configuration (`S3BucketToScan`). In our case please use the name from the S3 bucket that you have created early in this workshop or use one existing S3 bucket that you prefer.
   
   > **Note:** It also supports differents parameters to customize your installation, like Resource prefixes and optional KMS integration, for more details about these configurations check our Documentation.
   
   After adding the bucket name you will need to acknowledge and click on **Create Stack**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/7bc8c8b7-da52-4954-bcb5-f7bfeff05963" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/4777cb73-df80-4f7e-be4f-f6c671362f8c" />

9. After deploying the all-in-one stack in your AWS account, you must configure the scanner and storage stack Amazon Resource Names (ARNs) information in Cloud One console. The ARNs map a scanner stack to a storage stack, allowing them to be aware of each other.
   
   Go to **CloudFormation > Stacks > your all-in-one stack > Outputs** tab and copy the Value with these Key names here `ScannerStackManagementRoleARN` and `StorageStackManagementRoleARN` and paste the information into Cloud One - File Storage Security console.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/5705c705-b5a0-4704-b106-26024e2c6c3f" />

Then Click **Submit**. You see a couple of success messages at the bottom and the stack will show in the Stack Management too.

You have now completed the deployment of the All-in-One stack, so let’s test our deployment.

All the new objects that will be uploaded to your bucket that you define to be scan, now will be automatically scanning against malware and tagged as “malicious” or “no issues found”.

Here is a video with the steps-by-steps on how to deploy All-in-One stack for File Storage Security: [Deploying the File Storage Security All In One Stack
](https://youtu.be/_w1_k5W3tEQ?si=r5wNHPB9GyuWfVpE)
