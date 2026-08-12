---
title: "Build the Backend with AWS Lambda"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

## 5.4. Build the Backend with AWS Lambda

In this section, AWS Lambda is used to implement the backend business logic for the CloudMenu system.

CloudMenu currently uses three main Lambda functions:

- `createOrder`: Creates a new order.
- `getOrders`: Retrieves the list of orders.
- `updateOrderStatus`: Updates the status of an order.

The Lambda functions receive requests from Amazon API Gateway, process the required business logic, and interact with the `CloudMenuOrders` table in Amazon DynamoDB.

The main backend flow is:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

In this section, we will:

1. Review the Lambda IAM Execution Role.
2. Create and configure the CloudMenu Lambda functions.
3. Integrate Lambda with Amazon DynamoDB.
4. Test the Lambda functions.

After completing this section, the CloudMenu backend will be ready to integrate with Amazon API Gateway.