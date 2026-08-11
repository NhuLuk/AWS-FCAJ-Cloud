---
title: "Test the Lambda Functions"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4.4. </b>"
---

## 5.4.4. Test the Lambda Functions

After deploying the Lambda functions, each function should be tested before completing the integration with Amazon API Gateway.

AWS Lambda provides a **Test** feature that allows a sample Event to be created and the function to be executed directly from the AWS Management Console.

### Test createOrder

Create a Test Event with a structure similar to a request received from API Gateway.

After running the test, verify that:

- The function executes successfully.
- The response returns status `200`.
- A new Item is created in the `CloudMenuOrders` table.
- The initial order status is `PENDING`.

### Test getOrders

Run the `getOrders` function and review the response.

The function should return the Items currently stored in the `CloudMenuOrders` table.

### Test updateOrderStatus

Create a Test Event containing:

- `orderId` in `pathParameters`.
- The new order status in the request body.

Verify the status transition:

**PENDING → PREPARING → COMPLETED**

After the function executes, inspect the corresponding Item in DynamoDB and verify that `status`, `updatedAt`, and `completedAt` have been updated correctly.

<!-- FIGURE 5.4.4
An AWS Lambda Test Result screenshot can be added here if available.
This is optional because the full end-to-end system will be tested later in Section 5.8.
-->

After all Lambda functions work correctly, the backend is ready to be integrated with Amazon API Gateway in the next section.