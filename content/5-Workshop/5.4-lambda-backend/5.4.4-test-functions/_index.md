---
title: "Test the Lambda Functions"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4.4. </b>"
---

## 5.4.4. Test the Lambda Functions

After deploying the Lambda functions, each function should be tested to verify that it can process requests and interact with the `CloudMenuOrders` DynamoDB table correctly.

The main functions tested are:

- `createOrder`
- `getOrders`
- `updateOrderStatus`

### Test createOrder

The `createOrder` function is tested using a sample event containing order information.

A successful execution should:

- Return HTTP status `200`.
- Create a new order in the `CloudMenuOrders` table.
- Set the initial order status to `PENDING`.

### Test getOrders

The `getOrders` function is executed to retrieve the existing orders from DynamoDB.

A successful execution should return the stored orders as JSON.

### Test updateOrderStatus

The `updateOrderStatus` function is tested using an `orderId` and a new order status.

The function should update the corresponding order in DynamoDB and return HTTP status `200` when the operation succeeds.

After all Lambda functions are verified, the backend is ready to be integrated with Amazon API Gateway.