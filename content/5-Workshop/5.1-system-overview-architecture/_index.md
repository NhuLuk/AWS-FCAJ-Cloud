---
title: "System Overview and Architecture"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---

## 5.1. System Overview and Architecture

### Overview

CloudMenu is a QR code-based table ordering system that allows customers to access the ordering interface and place orders directly from their mobile devices without using paper menus or dedicated ordering devices.

Each table in the restaurant is assigned a unique QR code. When a customer scans the QR code, the system uses the information embedded in the code to identify the table number. Customers can then browse the menu, search for and select dishes, add items to the cart, submit an order, and track the order status.

The system provides three main interfaces:

- **Customer Interface:** Allows customers to access the system through a QR code, browse the menu, select dishes, place orders, and track order status.
- **Kitchen Interface:** Allows kitchen staff to view incoming orders, process them, and update their preparation status.
- **Manager Dashboard:** Allows managers to monitor the number of orders, revenue, order statuses, and other system statistics.

### Architecture

![AWS CloudMenu Architecture](/images/AWS_CloudMenu.png)

CloudMenu is deployed using a Serverless architecture on AWS. The frontend files are stored in **Amazon S3** and distributed to users through **Amazon CloudFront**. When users perform business operations, the frontend sends HTTP requests to **Amazon API Gateway**. API Gateway forwards the requests to **AWS Lambda**, where the business logic is processed. Lambda reads from or writes data to **Amazon DynamoDB** and returns the results to the frontend through API Gateway.

**AWS Identity and Access Management (IAM)** is used to control Lambda's access to DynamoDB and other required AWS resources.

This architecture allows CloudMenu to operate without maintaining a continuously running backend server while clearly separating the frontend, API, business logic, and data storage components.

In the future, the system can be extended with services and mechanisms such as **Amazon Cognito** for user authentication and authorization, **AWS WAF** for enhanced application protection, **CI/CD** for automated frontend and backend deployment, and additional monitoring mechanisms as the system grows.