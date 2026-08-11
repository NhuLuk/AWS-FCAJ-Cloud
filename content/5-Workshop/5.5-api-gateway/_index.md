---
title: "Build the API with Amazon API Gateway"
date: 2026-06-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

## 5.5. Build the API with Amazon API Gateway

In this section, Amazon API Gateway is used as the communication layer between the CloudMenu frontend and the AWS Lambda functions in the backend.

CloudMenu uses an HTTP API named:

`CloudMenuAPI`

The API receives HTTP requests from the Customer, Kitchen, and Manager interfaces and forwards each request to the corresponding Lambda function for processing.

The main request flow is:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

CloudMenu currently uses three main routes:

| Method | Route | Lambda Function | Purpose |
| :--- | :--- | :--- | :--- |
| `POST` | `/order` | `createOrder` | Create a new order. |
| `GET` | `/orders` | `getOrders` | Retrieve the order list. |
| `PUT` | `/orders/{orderId}` | `updateOrderStatus` | Update the status of an order. |

In this section, we will:

1. Create the `CloudMenuAPI` HTTP API.
2. Configure the application routes.
3. Connect each route to its corresponding Lambda function.
4. Test the API.

After completing this section, the CloudMenu frontend will be able to communicate with the backend through Amazon API Gateway.