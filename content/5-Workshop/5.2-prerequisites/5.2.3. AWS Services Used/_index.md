---
title: "AWS Services Used"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.2.3. </b>"
---

## 5.2.3. AWS Services Used

CloudMenu uses several AWS services to deploy the frontend, backend, and data storage components.

The main AWS services used in this workshop are:

| AWS Service | Role in CloudMenu |
| :--- | :--- |
| **Amazon S3** | Stores the frontend files and static application assets. |
| **Amazon CloudFront** | Distributes the frontend stored in S3 to users through a CDN. |
| **Amazon API Gateway** | Provides API endpoints and receives HTTP requests from the frontend. |
| **AWS Lambda** | Processes backend business logic without requiring a continuously running application server. |
| **Amazon DynamoDB** | Stores order data and other data required by the application. |
| **AWS IAM** | Controls permissions for Lambda and related AWS resources. |

The services are combined into two main flows.

### Frontend Distribution Flow

**User → Amazon CloudFront → Amazon S3**

Amazon S3 stores the frontend resources, while Amazon CloudFront distributes the content to the user's browser or mobile device.

### Data Processing Flow

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

The frontend sends HTTP requests to API Gateway. API Gateway forwards the requests to Lambda, where the business logic is processed. Lambda reads from or writes data to DynamoDB and returns the result to the frontend.

IAM provides the permissions required for Lambda to access DynamoDB and other related AWS resources.


After completing these prerequisites, the required AWS resources can be created and each CloudMenu component can be deployed.