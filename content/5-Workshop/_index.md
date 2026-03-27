---
title: "Workshop"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# Serverless Website Security Assessment Platform

#### Overview

**AWS Serverless Architecture** provides a way to build and run applications and services without managing infrastructure. It allows you to focus purely on your application code and business logic while inherently optimizing for high availability and cost-efficiency.

In this lab, you will learn how to build, deploy, and test a fully serverless web application designed to perform basic security assessments on target URLs.

You will utilize three core AWS services to create this secure, scalable platform: Amazon S3, Amazon CloudFront, and AWS Lambda. These services work together to deliver a tightly secured frontend and a dynamic backend processing engine:

+ **Amazon S3** - Acts as the secure storage repository for your static website assets (HTML, CSS, JS). Access to this bucket is strictly restricted so that content can only be served through the CDN.
+ **Amazon CloudFront** - Serves as the Content Delivery Network (CDN). It provides HTTPS encryption, global caching, and securely delivers the frontend from S3 to end-users utilizing Origin Access Control (OAC) to prevent direct public access to the bucket.
+ **AWS Lambda** - Functions as the backend computing engine. It receives assessment requests triggered from the frontend, executes the security scanning logic, and returns the generated results.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Access S3 from VPC](5.3-S3-vpc/)
4. [Access S3 from On-premises](5.4-S3-onprem/)
5. [VPC Endpoint Policies (Bonus)](5.5-Policy/)
6. [Clean up](5.6-Cleanup/)
