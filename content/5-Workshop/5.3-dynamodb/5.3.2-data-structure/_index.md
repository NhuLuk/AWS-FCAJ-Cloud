---
title: "Order Data Structure"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.3.2. </b>"
---

## 5.3.2. Order Data Structure

After creating the `CloudMenuOrders` table, CloudMenu order data is stored as Items in Amazon DynamoDB.

Each order is uniquely identified by the `orderId` attribute.

![CloudMenuOrders data model](/images/5-Workshop/5.3/dynamodb-data-model.png)

Based on the actual data used by CloudMenu, an order in the `CloudMenuOrders` table contains the following main attributes:

| Attribute | Description |
| :--- | :--- |
| `orderId` | Unique identifier of the order and the Partition Key of the table. |
| `tableNumber` | Number of the table that submitted the order. |
| `items` | List of dishes included in the order. |
| `totalAmount` | Total value of the order. |
| `status` | Current processing status of the order. |
| `createdAt` | Time when the order was created. |
| `updatedAt` | Time when the order was last updated. |
| `completedAt` | Time when the order was completed. |

### Order Status

During order processing, the order status follows the workflow:

**PENDING → PREPARING → COMPLETED**

Where:

- `PENDING`: The order has been submitted by the customer and is waiting for the kitchen to accept it.
- `PREPARING`: The kitchen has accepted the order and preparation is in progress.
- `COMPLETED`: The order preparation has been completed.

When a customer submits an order from the Customer Interface, the frontend sends a request to Amazon API Gateway. AWS Lambda processes the request and creates the corresponding order data in the `CloudMenuOrders` table.

The data creation flow is:

**Customer Interface → API Gateway → Lambda → CloudMenuOrders**

When kitchen staff accept or complete an order, the Kitchen Interface sends an update request. Lambda identifies the order using its `orderId` and updates the corresponding data in DynamoDB.

The update flow is:

**Kitchen Interface → API Gateway → Lambda → CloudMenuOrders**

By using `orderId` as the Partition Key, the backend can identify the order that needs to be processed during update operations.

In the next step, we will verify the actual Items stored in the `CloudMenuOrders` table.