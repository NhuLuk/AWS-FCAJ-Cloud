---
title: "CloudMenu Source Code"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.2.2. </b>"
---

## 5.2.2. CloudMenu Source Code

Before deploying the application to AWS, prepare the CloudMenu source code.

CloudMenu consists of frontend interfaces for different user roles and a backend responsible for processing business logic and interacting with the database.

### Frontend

The CloudMenu frontend provides three main interfaces:

- **Customer Interface:** Allows customers to access the system using a table QR code, browse the menu, select dishes, place orders, and track order status.
- **Kitchen Interface:** Allows kitchen staff to view incoming orders and update their processing status.
- **Manager Dashboard:** Allows managers to monitor orders, revenue, and system statistics.

The frontend resources will be uploaded to Amazon S3 and distributed to users through Amazon CloudFront.

<!-- FIGURE 5.2.2-1:
Use the existing CloudMenu Customer Interface screenshot. -->

![CloudMenu Customer Interface](/images/5-Workshop/5.2/customer-interface.png)

### Backend

The CloudMenu backend is implemented using a Serverless architecture. Requests from the frontend are sent to Amazon API Gateway and forwarded to AWS Lambda for processing.

Lambda handles the main business operations, including:

- Creating orders.
- Retrieving orders.
- Retrieving order information.
- Updating order status.
- Retrieving data required by the application interfaces.

Lambda reads and writes application data in Amazon DynamoDB.

The main backend request flow can be represented as:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

<!-- FIGURE 5.2.2-2:
Use the existing request-flow diagram showing
Browser/Phone → API Gateway → Lambda → DynamoDB.
-->

![CloudMenu Request Flow](/images/5-Workshop/5.2/request-flow.png)

Preparing both the frontend and backend source code before creating AWS resources helps ensure a smoother deployment process in the following sections.