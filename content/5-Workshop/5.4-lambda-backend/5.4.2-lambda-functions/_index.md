---
title: "CloudMenu Lambda Functions"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.4.2. </b>"
---

## 5.4.2. CloudMenu Lambda Functions

CloudMenu uses three AWS Lambda functions to process the main business operations of the system.

All functions are deployed with the **Python 3.13** runtime and use the **Zip** package type.

![Lambda functions](/images/5-Workshop/5.4/lambda-functions.png)

### createOrder

The `createOrder` function creates a new order.

The function receives order data from the request, including:

- `orderId`
- `tableNumber`
- `items`
- `totalAmount`
- `createdAt`

When an order is created, the backend automatically sets:

- `status` = `PENDING`
- `updatedAt` = the order creation time
- `completedAt` = `None`

The order is then written to the `CloudMenuOrders` table using:

`put_item()`

Processing flow:

**Customer → API Gateway → createOrder → DynamoDB**

If a required field is missing, the function returns HTTP status `400`. If an internal processing error occurs, it returns status `500`.

### getOrders

The `getOrders` function retrieves the current list of orders from the `CloudMenuOrders` table.

The function uses:

`table.scan()`

to read Items from DynamoDB.

Because DynamoDB can return values using the `Decimal` type, the function uses a `DecimalEncoder` to convert Decimal values into JSON-compatible numbers.

Processing flow:

**Kitchen / Manager → API Gateway → getOrders → DynamoDB**

The result is returned as JSON for use by the Kitchen and Manager interfaces.

### updateOrderStatus

The `updateOrderStatus` function updates the status of an order.

The function retrieves `orderId` from `pathParameters` and reads the new status from the request body.

The allowed statuses are:

- `PENDING`
- `PREPARING`
- `COMPLETED`

The function updates:

- `status`
- `updatedAt`

If the new status is `COMPLETED`, the function also updates:

- `completedAt`

The data is updated using:

`update_item()`

and the function uses the condition:

`attribute_exists(orderId)`

to ensure that the target order exists in DynamoDB.

Processing flow:

**Kitchen → API Gateway → updateOrderStatus → DynamoDB**

If the order does not exist, the function returns status `404`. Invalid requests return `400`, while internal errors return `500`.