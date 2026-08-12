---
title: "AWS Account and Environment"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.2.1. </b>"
---

## 5.2.1. AWS Account and Environment

### AWS Account

An AWS account with access to the AWS Management Console is required to complete this workshop.

The AWS services used to deploy CloudMenu include:

- Amazon DynamoDB
- AWS Lambda
- Amazon API Gateway
- Amazon S3
- Amazon CloudFront
- AWS Identity and Access Management (IAM)

After signing in to the AWS Management Console, use the search bar at the top of the console to access the required services.

![AWS Management Console](/images/5-Workshop/5.2/aws-console.png)

### AWS Region

Regional resources used in this workshop should be deployed consistently within the selected AWS Region to simplify configuration and integration between services.

Before creating resources, verify the selected Region in the AWS Management Console and use the appropriate Region throughout the deployment process.

### Browser and Testing Device

A web browser is required to:

- Access the AWS Management Console.
- Verify the deployed frontend.
- Send requests to the backend through the CloudMenu interface.
- Test the Customer, Kitchen, and Manager interfaces.

A mobile device can also be used to scan the table QR code and test the Customer Interface on a mobile screen.