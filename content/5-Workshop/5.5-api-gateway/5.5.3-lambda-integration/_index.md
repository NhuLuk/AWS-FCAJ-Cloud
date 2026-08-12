---
title: "Integrating API Gateway with AWS Lambda"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.5.3. </b>"
---

## 5.5.3. Integrating API Gateway with AWS Lambda

After creating the Routes, each Route needs to be connected to the corresponding Lambda Function that performs the required backend operation.

In Amazon API Gateway, the connection between a Route and a backend service is called an **Integration**.

CloudMenu uses AWS Lambda as the backend integration for its HTTP API.

### Step 1: Open Integrations

In `CloudMenuAPI`, select:

**Develop → Integrations**

CloudMenu has three Lambda Integrations corresponding to the three main backend operations.

![CloudMenuAPI Integrations](/images/5-Workshop/5.5/api-integrations.png)

The following configuration is used:

| Route | Integration | Lambda Function |
| :---: | :--- | :--- |
| `POST /order` | Lambda Integration | `createOrder` |
| `GET /orders` | Lambda Integration | `getOrders` |
| `PUT /orders/{orderId}` | Lambda Integration | `updateOrderStatus` |

### Step 2: Integrate createOrder with POST /order

Select the Route:

`POST /order`

Then select the corresponding Lambda Integration:

`createOrder`

![POST order Integration](/images/5-Workshop/5.5/post-order-integration.png)

When a Customer sends a request to `POST /order`, Amazon API Gateway forwards the request to the `createOrder` Lambda Function.

The Function receives the order data from the request, processes the required information, and stores the new order in the `CloudMenuOrders` Amazon DynamoDB table.

Processing flow:

```text
POST /order
     ↓
Amazon API Gateway
     ↓
createOrder
     ↓
DynamoDB put_item()
```

### Step 3: Integrate getOrders with GET /orders

The Route:

`GET /orders`

is connected to the Lambda Function:

`getOrders`

When a request is received from the frontend, Amazon API Gateway forwards it to the `getOrders` Function.

The Function uses the `scan()` operation to retrieve the existing Items from the `CloudMenuOrders` table and returns the order list to API Gateway.

Processing flow:

```text
GET /orders
     ↓
Amazon API Gateway
     ↓
getOrders
     ↓
DynamoDB scan()
```

This Route is used by interfaces that need to retrieve order information, such as the Kitchen Interface and Dashboard.

### Step 4: Integrate updateOrderStatus with PUT /orders/{orderId}

The Route:

`PUT /orders/{orderId}`

is connected to the Lambda Function:

`updateOrderStatus`

In this Route, `{orderId}` is a Path Parameter used to identify the order that needs to be updated.

For example:

```text
PUT /orders/ORD003
```

Amazon API Gateway forwards the request and the `orderId` value to the `updateOrderStatus` Lambda Function.

The Function retrieves `orderId` from the Path Parameter and the new status from the Request Body. It then uses the `update_item()` operation to update the corresponding Item in the `CloudMenuOrders` table.

Processing flow:

```text
PUT /orders/{orderId}
          ↓
Amazon API Gateway
          ↓
updateOrderStatus
          ↓
DynamoDB update_item()
```

### Verify Routes and Integrations

After completing the configuration, each Route should be checked to ensure that it is connected to the correct Lambda Integration.

The final CloudMenu configuration is:

```text
POST /order
    └── createOrder
            └── DynamoDB put_item()

GET /orders
    └── getOrders
            └── DynamoDB scan()

PUT /orders/{orderId}
    └── updateOrderStatus
            └── DynamoDB update_item()
```

After the Integrations are successfully configured, Amazon API Gateway can receive HTTP requests from the frontend and forward them to the corresponding Lambda Functions for processing.

In the next step, the APIs will be tested to verify that the integration between Amazon API Gateway, AWS Lambda, and Amazon DynamoDB works correctly.