---
title: "Verify the Data"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.3.3. </b>"
---

## 5.3.3. Verify the Data

After CloudMenu has created and processed orders, we can inspect the data stored in Amazon DynamoDB to verify that data creation and update operations are working correctly.

### Step 1: Open the CloudMenuOrders Table

In the AWS Management Console, navigate to:

**Amazon DynamoDB → Explore items**

Select the table:

`CloudMenuOrders`

Then run the operation to display the existing Items in the table.

### Step 2: Verify the Items

DynamoDB displays the orders stored in `CloudMenuOrders`.

![Data stored in the CloudMenuOrders table](/images/5-Workshop/5.3/order-items.png)

The results allow us to verify attributes including:

- `orderId`
- `completedAt`
- `createdAt`
- `items`
- `status`
- `tableNumber`
- `totalAmount`
- `updatedAt`

Each row represents an order in CloudMenu.

For example, `orderId` distinguishes individual orders, `tableNumber` identifies the table that submitted the order, `totalAmount` stores the total order value, and `status` represents the current processing state.

### Step 3: Compare the Data with CloudMenu

The DynamoDB data can be compared with the CloudMenu user interfaces.

When a customer submits an order:

**Customer → API Gateway → Lambda → DynamoDB**

A corresponding Item is created in the `CloudMenuOrders` table.

When kitchen staff update an order:

**Kitchen → API Gateway → Lambda → DynamoDB**

The `status` attribute of the corresponding order is updated.

The Manager Dashboard uses data returned by the backend to provide information such as the number of orders, revenue, and order processing status.

The presence of Items containing the expected order information in `CloudMenuOrders` confirms that the data storage layer is working and is ready to support the CloudMenu backend functions.

In the next section, we will deploy AWS Lambda functions to process the application business logic and interact with the `CloudMenuOrders` table.