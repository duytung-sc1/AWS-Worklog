---
title : "Testing your Deployment"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 4.3.1 </b> "
---

# Testing your Deployment

## Generating your First Detection

To test your deployment, you’ll need to generate a malware detection using the eicar file. Follow the instructions below.
This video here also demostrate the steps-by-steps if you prefer to check additional details before following the steps.
[Trend Micro Cloud One](https://youtu.be/2WDpQ7KgjRo?si=4a3GP_3EHVrXXYdG)

1. In the AWS console, open AWS CloudShell in a new tab.
To download the eicar test file, in CloudShell run this command: `wget https://secure.eicar.org/eicar.com.txt`
<img width="800" alt="image" src="https://github.com/user-attachments/assets/9e880df0-6297-4d6f-bbc3-085e41e6cb29" />

Upload the eicar file to the S3 bucket created previosuly: `aws s3 cp eicar.com.txt s3://name-of-bucket-goes-here/eicar.txt`
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3df5c887-9838-42d5-9b6e-561518f0aa22" />

2. After successful upload navigate to S3 and find your S3 bucket that you defined to be scanned by File Storage Security.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/b87250f2-b8bc-45f3-99ac-b4b6f461ba76" />

3. Select the uploaded eicar file.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/c41502b3-bfec-423d-914c-b4eb45bb422f" />

4. Examine the tags applied from the scan results.
Scroll to the Tags section, you will see the details like the example below:
<img width="800" alt="image" src="https://github.com/user-attachments/assets/50575dee-1c10-446d-8430-bc14e71f21aa" />

Look for the following tags:
* fss-scan-date "date_and_time"
* fss-scan-result "malicious"
* fss-scanned "true"

The tags indicate that File Storage Security scanned the file and tagged it correctly as malware. You have now tested your File Storage Security deployment.

You could also check in the Cloud One - File Storage Security dashboard to look for how many files have been scanned and recognized as malicious.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f0006b00-469c-4dc5-ba9a-0ba75ec69f99" />

You could do the same process with a safe file and see how Cloud One - File Storage Security will be tagging it in the object.
