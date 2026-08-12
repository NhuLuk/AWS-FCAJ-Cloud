---
title: "Lambda Function Testing"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4.4. </b>"
---

## 5.4.4. Lambda Function Testing

After deploying the Lambda Functions and configuring their permissions to access Amazon DynamoDB, each Function should be tested to verify that the CloudMenu backend can process data correctly before performing complete testing through Amazon API Gateway.

AWS Lambda provides a **Test** feature that allows a Test Event to be created and the Function to be executed directly from the AWS Management Console.

In this section, the three main Lambda Functions of CloudMenu are tested:

- `createOrder`
- `getOrders`
- `updateOrderStatus`

---

### Testing createOrder

The `createOrder` Function is responsible for receiving order information and creating a new Item in the `CloudMenuOrders` table.

To perform the test, navigate to:

**AWS Lambda → Functions → createOrder → Test**

Create a Test Event containing sample order data. The Event is structured similarly to the request that the Function receives from Amazon API Gateway.

Example:

```json
{
  "body": "{\"orderId\":\"TEST001\",\"tableNumber\":\"01\",\"items\":[{\"name\":\"Cơm chiên\",\"price\":50000,\"quantity\":1}],\"totalAmount\":50000}"
}