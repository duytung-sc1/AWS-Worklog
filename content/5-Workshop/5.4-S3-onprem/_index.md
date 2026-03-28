---
title : "Automation and Notification"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 4.4. </b> "
---


##Automate deployment and scan notification.

After Cloud One - File Storage Security completes a scan, the scan results are tagged to the file and published to Amazon SNS topic `ScanResultTopic`.

If you want to do more with the results, you’ll have to create or add a post-action to take place after the scan. We provide many samples of integrations that can be done with Cloud One File Storage Security in our GitHub page, one of the most used Post-Actions is the ability to send clean files to one Amazon S3 bucket (promote) and send malicious files to another Amazon S3 bucket (quarantine). We also provide an API to help with creating your own post-scan actions.



Let’s start creating the post-actions for automation and monitoring.
