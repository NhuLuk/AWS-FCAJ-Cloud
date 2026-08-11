---
title: "Week 7 Worklog"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives

- Complete the integration of the CloudMenu system components on AWS.
- Deploy the frontend to Amazon S3 and distribute the content through Amazon CloudFront.
- Connect the frontend with Amazon API Gateway, AWS Lambda, and Amazon DynamoDB.
- Test the complete ordering workflow from customers to the kitchen and resolve integration issues.

**Duration:** 03/08/2026 - 07/08/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Complete the main components of the **CloudMenu** system <br> + Review the customer and Kitchen interfaces <br> + Verify the Lambda Functions and APIs <br> + Check the data structure in the `CloudMenuOrders` table | 03/08/2026 | 03/08/2026 | - |
| 2 | - Deploy the CloudMenu frontend to **Amazon S3** <br> + Upload HTML, CSS, JavaScript, and image files <br> + Verify the structure and paths of frontend files <br> + Configure Amazon S3 as the Origin for CloudFront | 04/08/2026 | 04/08/2026 | [https://aws.amazon.com/s3/](https://aws.amazon.com/s3/) |
| 3 | - Distribute the frontend through **Amazon CloudFront** <br> + Configure a CloudFront Distribution <br> + Access CloudMenu through the CloudFront Domain <br> + Perform CloudFront Invalidation after frontend updates <br> + Test the interface on desktop and mobile devices | 05/08/2026 | 05/08/2026 | [https://aws.amazon.com/cloudfront/](https://aws.amazon.com/cloudfront/) |
| 4 | - Integrate the components of the **CloudMenu** system <br> + Connect the frontend to Amazon API Gateway <br> + Verify the API Gateway → AWS Lambda → Amazon DynamoDB flow <br> + Check CORS configuration and resolve frontend API request issues <br> + Verify order data stored in DynamoDB | 06/08/2026 | 06/08/2026 | [https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/) |
| 5 | - Test the complete **CloudMenu** system <br> + Scan a QR Code to access the correct table <br> + Select menu items and submit an order from the customer interface <br> + Verify the order on the Kitchen interface <br> + Update the status from `PENDING` → `PREPARING` → `COMPLETED` <br> + Verify that the updated order status is displayed to the customer | 07/08/2026 | 07/08/2026 | - |

---

### Week 7 Achievements

- Completed the integration of the main CloudMenu system components.
- Deployed the frontend to Amazon S3 and distributed the website through Amazon CloudFront.
- Successfully connected the frontend with Amazon API Gateway, AWS Lambda, and Amazon DynamoDB.
- Identified and resolved issues related to CORS and frontend-backend communication.
- Successfully tested table identification using QR Codes and URL parameters.
- Completed the order processing workflow from customer order submission to kitchen processing and order completion.