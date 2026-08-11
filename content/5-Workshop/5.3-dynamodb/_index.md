---
title: "Database with Amazon DynamoDB"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

## 5.3. Database with Amazon DynamoDB

In this section, Amazon DynamoDB is used as the database for the CloudMenu system. DynamoDB stores order data and provides the data storage layer for order placement, order processing, and system statistics.

CloudMenu does not allow the frontend to access the database directly. Instead, requests from the user interfaces are sent to Amazon API Gateway, where AWS Lambda processes the business logic and performs read or write operations on DynamoDB.

The main data flow is:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

In this section, we will:

1. Create the `CloudMenuOrders` table in Amazon DynamoDB.
2. Review the order data structure stored in the table.
3. Verify the data created and updated while CloudMenu is running.

After completing this section, the DynamoDB table will be ready to integrate with the AWS Lambda functions used by CloudMenu.