---
title : "Slack Integration"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 4.4.3. </b> "
---

## Overview

This is an **optional lab** if you would like to integrate notifications from Cloud One - File Storage Security into your Slack workspace. This integration uses a Lambda function to send a Slack message every time a new detection occurs.

---

## Requirements

### 1. Configure Slack Webhook App
First, you need to set up an Incoming Webhook in your Slack workspace:

1. Create a specific Slack Channel to receive notifications (e.g., `#fss-alerts`).
2. Go to the **Slack App Directory** and search for **Incoming WebHooks**.
3. Click on **Incoming WebHooks**, then click **Add to Slack**.
4. Choose the channel you created in step 1.
5. Click **Add Incoming WebHooks integration**.
6. **Copy the Webhook URL**: Save this for later; you will need it for the deployment.
7. (Optional) Customize the **Name** (e.g., `FSS-Bot`) and **Icon**.
8. Click **Save Settings**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/08fb51a9-8e5c-4fc0-a739-904918d02d84" />

> For more details on creating Incoming Webhooks, refer to the [Addtional Information](https://slack.com/help/articles/115005265063-Incoming-webhooks-for-Slack).

### 2. Find the ScanResultTopic ARN
1. In the AWS console, go to **Services > CloudFormation**.
2. Select your **Storage Stack** (nested under the All-in-One stack).
3. Click on the **Resources** tab.
4. Scroll down to locate the `ScanResultTopic` Logical ID.
5. Copy the **ARN** to a temporary location.
   * *Example:* `arn:aws:sns:us-east-1:000000000000:FileStorageSecurity-StorageStack-ScanResultTopic-N8DD2JH1GRKF`
<img width="800" alt="image" src="https://github.com/user-attachments/assets/9e9af668-ca62-4cfd-896b-a8823d935ef4" />

---

## Deploying the Slack Plugin

We will use the AWS Serverless Application Repository to deploy the integration.

1. Open the [CloudOne-FSS-Plugin-Slack application](https://serverlessrepo.aws.amazon.com/applications/us-east-1/024368254241/CloudOne-FSS-Plugin-Slack) in the AWS Lambda Console.
2. Click **Deploy**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/8ec6d16b-0ba5-4273-918b-ed64d95b40b2" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/9095b27d-1582-4353-9f9b-7486f490210d" />

3. Fill in the **Application settings** parameters:
   * **ScanResultTopicARN**: Paste the ARN you copied in the previous section.
   * **SlackChannel**: Enter the name of your Slack channel (e.g., `#fss-alerts`).
   * **SlackURL**: Paste the Webhook URL you generated in Slack.
   * **SlackUsername**: Enter the name you want the bot to use when posting.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/7fb5d44c-ba55-42f7-af8a-93288d46d7cb" />
<img width="1635" height="431" alt="image" src="https://github.com/user-attachments/assets/3271d6b8-2942-41a9-a123-00aad4a7f3f5" />

4. Ensure the application reaches create complete.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/0baef418-43e0-4ccc-8d25-4450898d9d21" />

5. Click **Deploy**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/cc331d2f-57ab-494f-a5d9-e55bbd9be4d9" />

Wait a few minutes for the status to reach **Create complete**.

---

## Test the Integration

To verify that the Slack notifications are working correctly:

1. Generate a new malware event by uploading the `eicar` test file to your **Scanning Bucket** (as performed in the previous labs).
2. Wait 15–30 seconds for the scan to complete.
3. Check your Slack channel. You should receive a notification containing the detection details, such as the file name, bucket name, and the type of malware found.

> For more advanced configuration or source code, visit our [GitHub Repository](https://github.com/trendmicro/cloudone-filestorage-plugins/tree/master/post-scan-actions/aws-python-slack-notification).
