---
title: "Configure Routes"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.5.2. </b>"
---

## 5.5.2. Configure Routes

After creating `CloudMenuAPI`, the next step is to define Routes that determine how API Gateway handles HTTP requests from the frontend.

An HTTP API Route is primarily defined by:

**HTTP Method + Resource Path**

For example:

`GET /orders`

CloudMenu uses three Routes to process order-related operations.

| Method | Route | Purpose |
| :---: | :--- | :--- |
| `POST` | `/order` | Create a new order. |
| `GET` | `/orders` | Retrieve the list of orders. |
| `PUT` | `/orders/{orderId}` | Update the status of an order. |

### Step 1: Open Routes

In `CloudMenuAPI`, navigate to:

**Develop → Routes**

The existing Routes of the system are displayed.

![CloudMenuAPI Routes](/images/5-Workshop/5.5/api-routes.png)

### Step 2: Configure POST /order

The Route:

`POST /order`

is used when a customer submits a new order.

The request body contains order information such as the table number, selected items, and total order value.

The request is then forwarded to the `createOrder` Lambda function.

Processing flow:

**Customer → POST /order → createOrder**

### Step 3: Configure GET /orders

The Route:

`GET /orders`

is used to retrieve the existing order list.

This Route is used by interfaces that need to read order information, particularly the Kitchen Interface and Manager Dashboard.

Processing flow:

**Kitchen / Manager → GET /orders → getOrders**

### Step 4: Configure PUT /orders/{orderId}

The Route:

`PUT /orders/{orderId}`

is used to update the status of a specific order.

`{orderId}` is a Path Parameter containing the identifier of the order to be updated.

For example:

`PUT /orders/ORDER001`

The Lambda function uses the `orderId` Path Parameter to identify the corresponding Item in DynamoDB.

Processing flow:

**Kitchen → PUT /orders/{orderId} → updateOrderStatus**

After the Routes are created, each Route must be attached to its corresponding Lambda Integration.