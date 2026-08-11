---
title: "Integrate API Gateway with AWS Lambda"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.5.3. </b>"
---

## 5.5.3. Integrate API Gateway with AWS Lambda

After creating the Routes, each Route must be connected to the Lambda function responsible for the corresponding business operation.

In Amazon API Gateway, the connection between a Route and a backend is called an **Integration**.

CloudMenu uses AWS Lambda as its backend integration.

### Step 1: Open Integrations

In `CloudMenuAPI`, navigate to:

**Develop → Integrations**

CloudMenu has three Lambda integrations corresponding to the three backend operations.

![CloudMenuAPI Integrations](images/api-integrations.png)

The following configuration is used:

| Route | Integration | Lambda Function |
| :--- | :--- | :--- |
| `POST /order` | Lambda Integration | `createOrder` |
| `GET /orders` | Lambda Integration | `getOrders` |
| `PUT /orders/{orderId}` | Lambda Integration | `updateOrderStatus` |

### Step 2: Attach createOrder to POST /order

Select the Route:

`POST /order`

Then select the Integration associated with:

`createOrder`

![POST order Integration](images/post-order-integration.png)

When a customer sends a request to `POST /order`, API Gateway forwards the request to `createOrder`.

The function processes the request and writes the new order to the `CloudMenuOrders` table.

Processing flow:

**POST /order → createOrder → DynamoDB put_item()**

### Step 3: Attach getOrders to GET /orders

The Route:

`GET /orders`

is attached to:

`getOrders`

When the request is received, the function reads data from `CloudMenuOrders` and returns the order list to API Gateway.

Processing flow:

**GET /orders → getOrders → DynamoDB scan()**

### Step 4: Attach updateOrderStatus to PUT /orders/{orderId}

The Route:

`PUT /orders/{orderId}`

is attached to:

`updateOrderStatus`

The function retrieves `orderId` from the Path Parameter and the new status from the Request Body.

It then updates the corresponding Item in DynamoDB.

Processing flow:

**PUT /orders/{orderId} → updateOrderStatus → DynamoDB update_item()**

### Verify Routes and Integrations

After completing the configuration, verify each Route to ensure that it is attached to the correct Integration.

The final CloudMenu configuration is:

```text
POST /order
    └── createOrder

GET /orders
    └── getOrders

PUT /orders/{orderId}
    └── updateOrderStatus