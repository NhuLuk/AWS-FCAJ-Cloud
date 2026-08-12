---
title: "Proposal"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Proposal for Deploying CloudMenu on AWS

## 1. Project Overview

CloudMenu is a table-side online ordering system that allows customers to scan a QR Code assigned to each table using their mobile devices to access the menu, select dishes, and submit orders directly to the kitchen.

The system consists of three main user groups:

- Customers: Browse the menu, search/filter dishes, manage the shopping cart, place orders, and track order status.
- Kitchen Staff: Receive orders, view order details, and update the food preparation status.
- Admin/Manager: Monitor a statistical Dashboard showing the total number of orders, revenue, order status, revenue by table, and the most frequently ordered dishes.

CloudMenu is proposed to be deployed using a Serverless Architecture on AWS to reduce operational costs, provide scalability, and accommodate the system's variable traffic patterns.

## 2. Problems and Solution

### Current Problems

The traditional ordering process may present several limitations:

- Customers have to wait for staff to take their orders.
- Errors may occur when recording dishes and quantities manually.
- Kitchen staff may have difficulty monitoring and updating order statuses in real time.
- Managers may find it difficult to consolidate revenue and business statistics.
- The system needs to handle increased traffic during peak hours.

### Solution

CloudMenu uses a dedicated QR Code for each table, allowing customers to directly access the menu and place orders. Requests are processed through a Serverless Architecture:

QR Code → Frontend → API Gateway → Lambda → DynamoDB

The proposed solution provides the following benefits:

- Automatically identifies the table number through the QR Code.
- Reduces manual operations and minimizes ordering errors.
- Centralizes order data in DynamoDB.
- Provides automatic scalability through AWS Lambda.
- Provides a Dashboard for managers to monitor business activities.

## 3. Solution Architecture

![CloudMenu AWS architecture](/images/AWS_CloudMenu.png)

The main components include:

Frontend
- Amazon S3: Stores the HTML, CSS, and JavaScript files of CloudMenu.
- Amazon CloudFront: Distributes frontend content through a CDN to reduce access latency.

Backend
- Amazon API Gateway: Provides RESTful APIs and receives requests from the frontend.
- AWS Lambda: Handles business logic such as retrieving menus, creating orders, updating order statuses, and retrieving statistical data.
- Amazon DynamoDB: Stores menu, order, table, and order status data.

Security: AWS IAM: Manages access permissions between AWS services and controls administrative access to the system.

## 4. Timeline (8 Weeks)

The CloudMenu project is carried out over eight weeks, progressing from learning AWS services and Serverless architecture to developing, deploying, integrating, and testing the complete system.

* **Week 1 (22/06 - 26/06) — AWS Cloud and Serverless Fundamentals**

  * Become familiar with the AWS Cloud platform and its main service categories.
  * Learn about Serverless architecture and the main components of a web application.
  * Explore Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, and AWS IAM.
  * Build foundational knowledge for the development of the CloudMenu system.

* **Week 2 (29/06 - 03/07) — IAM, Amazon S3, and Amazon CloudFront**

  * Learn about AWS IAM and access management principles.
  * Learn about Amazon S3 and how it can be used to store frontend resources.
  * Learn about Amazon CloudFront and content delivery over HTTPS.
  * Practice deploying a static website using Amazon S3 and Amazon CloudFront.

* **Week 3 (06/07 - 10/07) — Amazon DynamoDB and Data Design**

  * Learn about NoSQL databases and Amazon DynamoDB.
  * Become familiar with Table, Item, Attribute, and Partition Key.
  * Practice creating, reading, and updating data in DynamoDB.
  * Design the order data structure for the CloudMenu system.
  * Define `orderId` as the Partition Key for the `CloudMenuOrders` table.

* **Week 4 (13/07 - 17/07) — AWS Lambda and Serverless Backend**

  * Learn about AWS Lambda and the Function as a Service (FaaS) model.
  * Practice creating, configuring, and testing Lambda Functions.
  * Connect AWS Lambda to Amazon DynamoDB using the AWS SDK for Python (`boto3`).
  * Develop Lambda Functions for creating orders, retrieving orders, and updating order status.

* **Week 5 (20/07 - 24/07) — Amazon API Gateway and REST API**

  * Learn about Amazon API Gateway, REST APIs, and HTTP methods.
  * Build APIs to support the CloudMenu order processing workflow.
  * Integrate Amazon API Gateway with AWS Lambda and Amazon DynamoDB.
  * Configure CORS and connect the frontend to the system backend.

* **Week 6 (27/07 - 31/07) — CloudMenu System Analysis and Development**

  * Analyze requirements and identify the main features of CloudMenu.
  * Develop the table identification and ordering mechanism using QR Codes.
  * Build the Customer Interface with menu browsing, shopping cart, and order submission features.
  * Build the Kitchen Interface for monitoring orders and updating order status.

* **Week 7 (03/08 - 07/08) — CloudMenu Deployment and Integration on AWS**

  * Complete the main components of the CloudMenu system.
  * Deploy the frontend to Amazon S3 and distribute it through Amazon CloudFront.
  * Integrate the frontend with Amazon API Gateway, AWS Lambda, and Amazon DynamoDB.
  * Test the workflow from customer order submission to kitchen processing and order completion.

* **Week 8 (10/08 - 15/08) — CloudMenu Completion and Program Summary**

  * Develop and complete the system's statistical Dashboard.
  * Complete the order status and processing-time tracking features.
  * Test, debug, and finalize the CloudMenu system.
  * Complete the architecture diagrams, processing-flow diagrams, README, Workshop, and report documentation.
  * Summarize the knowledge and skills gained throughout the First Cloud AI Journey program.

## 5. Budget

CloudMenu is designed to use AWS Serverless services and the AWS Free Tier during the testing phase to minimize deployment costs.

Main Services and Estimated Costs

| AWS Service | Component / Usage | Cost (USD/month) |
|---|---|---:|
| Amazon S3 | Frontend hosting + static assets | $0 - $3 |
| Amazon CloudFront | CDN + data transfer | $2 - $15 |
| Amazon API Gateway | REST API + API requests | $0 - $10 |
| AWS Lambda | Backend functions + invocations | $0 - $8 |
| Amazon DynamoDB | Menu + Orders + Table data | $0 - $10 |
| AWS IAM | Users / Roles / Policies | No direct AWS charge |
| **TOTAL AWS COST** |  | **$2 - $50** |

Cost control recommendations:
- Take advantage of the AWS Free Tier during development and testing.
- Configure AWS Budgets to receive alerts when costs exceed the defined budget threshold.
- Optimize S3 storage and use Lifecycle Policies as data volume increases.
- Regularly check and remove unused testing resources.
- Prioritize a Serverless Architecture to avoid costs associated with continuously running servers.

## 6. Risks

**Unexpected Increase in AWS Costs**
  *Mitigation:* Configure AWS Budgets and monitor costs through AWS Cost Management.

**Sudden Increase in Traffic**
  *Mitigation:* Use a Serverless Architecture with automatic scalability to handle traffic spikes.

**Unauthorized API Access**
  *Mitigation:* Use IAM and appropriate authentication and authorization mechanisms.

**Data Loss or Accidental Data Deletion**
  *Mitigation:* Enable Backup and Point-in-Time Recovery for DynamoDB.

**QR Code Used for the Wrong Table**
  *Mitigation:* Assign a unique table identifier to each QR Code and validate the table information when creating an order.

**Duplicate Order Submissions**
  *Mitigation:* Implement mechanisms to detect and handle duplicate requests.