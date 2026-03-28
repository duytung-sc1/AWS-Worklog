---
title : "Deploy Lifecycle"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 4.4.4. </b> "
---

## Automate Storage Stack deployment to new S3 buckets.

In this integration, we will deploy a CloudFormation template to ensure File Storage Security monitors any new S3 buckets that are created. Additionally anytime a S3 bucket resource is terminated, this template will automatically remove all deployed security resources to monitor the bucket.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/6b36a2df-ea3f-402c-9921-025fec2c965d" />

---

## Prerequisites

### 1. Obtain your Cloud One Account Region.
* Sign into **Cloud One**.
* Select **Account Settings**.
* Copy down the **Region**: e.g `us-1`.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/58769d11-9a16-45fd-99bc-422f8b3c812d" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/574d2a10-cd42-4254-abd1-871c7c409896" />

### 2. Create a new API Key in Cloud One.
* Under **Account Settings** in Cloud One, select **API Keys** from the left-hand menu.
* Click **New**.
* **API Key Alias**: `immersion_day`
* **Description**: Optional.
* **Role**: `Full Access`
* **Language**: preferred language.
* **Timezone**: preferred timezone.
* Click **Next**.
* **Copy down your API Key** in a safe place.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/c6fc1793-82df-4033-aab8-c98172dba78e" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/02e715fc-6bd0-4aa9-979a-8ae9fc69b238" />

### 3. Obtain the name of the Scanner Stack and the Scanner Stacks SQS URL.
* Navigate to **AWS CloudFormation**.
* Locate and select your deployed **Scanner Stack**.
* Click the tab named **Outputs**.
* Copy down your **Scanner Stacks name**.
* Locate the key **ScannerQueueURL**.
* Copy down the **ScannerQueueURL** value.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/1ad88048-ccb5-4fe3-93e3-33c801f2f17e" />

---

## Deploy the CloudFormation template below
[Launch Stack](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=c1fss-lifecycle-workshop&templateURL=https://aws-workshop-c1as-cft-templates.s3.amazonaws.com/fss-lifecycle.yml)

1. **Fill in the required template parameters with the values copied down previously:**
   * **C1API**: paste your cloud one api key.
   * **C1RegionEndpoint**: paste your cloud one account region.
   * **SQSURL**: paste the sanner stack’s sqs url value.
   * **StackName**: paste the name of your deployed Scanner stack.
   * Click **Next**.
   * Optional - Configure tags if desired.
   * Click **Next**.
   * Check the box at the bottom to acknowledge IAM resource creation.
   * Click **Create Stack**.
LaunchStack()
<img width="800" alt="image" src="https://github.com/user-attachments/assets/bcb59452-6bdc-42e9-a3e3-b97e2e4404cd" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/fe888208-87fa-4fb9-975d-6fed593a6a68" />

2. **Monitor the stack deployment** until it reaches status: **CREATE_COMPLETE**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3b537097-0eca-45bf-bad4-f0ae2919a2db" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/8d93a959-63cb-477e-b920-c9b107397ad6" />

---

## Test the automation

### Create a new S3 bucket
> [CLICK HERE](https://aws.amazon.com/video/watch/5c76e13b7fe/) - Step by step instruction to create a S3.

---
<img width="800" alt="image" src="https://github.com/user-attachments/assets/378851dc-a6dc-4450-8ac2-78dc65a4031c" />

### Verify and Clean Up

1. **Monitor CloudFormation**: After the S3 bucket has been created, monitor CloudFormation to see the new Storage stack being deployed automatically. Wait for the stack to reach **CREATE_COMPLETE**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/91866103-4289-4d85-841d-3a716ad12e17" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/0f858c97-be8d-4eab-a822-26db7fddb59c" />

2. **Check Cloud One**: Once the stack reaches create complete, check your Cloud One File Storage Console for the newly monitored bucket.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/6dfb554a-938e-43e7-8431-cc8e9c348d83" />

3. **Delete the S3 bucket**:
   * In AWS navigate to **S3**.
   * Locate and select the bucket created in the last step.
   * Click **Delete**.
   * Confirm deletion by pasting the bucket name and clicking **Delete bucket**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/15e78b50-005e-4618-882e-285148120ee0" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/6f2a2e85-fe2b-4272-b912-830f06fdaad7" />

4. **Monitor Removal**: Check **CloudFormation** to see the stack being removed.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/deed64a2-786f-47e4-baa8-06734c262211" />

5. **Final Check**: Once the stack is deleted, check your Cloud One File Storage Console to confirm the bucket is no longer listed as monitored.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/fc91869b-ea2a-4eb9-942a-492760cb2d98" />

