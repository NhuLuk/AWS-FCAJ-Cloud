---
title: "API Testing"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.5.4. </b>"
---

## 5.5.4. API Testing

After completing the Route configuration and Lambda Integrations, the APIs are tested to verify that Amazon API Gateway can forward requests to the correct Lambda Functions and that the backend functions of CloudMenu work properly.

In CloudMenu, three main APIs are tested using Postman:

| Method | Endpoint | Function |
| :---: | :--- | :--- |
| `POST` | `/order` | Create a new order |
| `GET` | `/orders` | Retrieve the order list |
| `PUT` | `/orders/{orderId}` | Update the order status |

---

### 1. Testing POST /order

The `POST /order` API is used to create a new order.

In Postman, select the **POST** method and enter the endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/order
```

In the **Body** section, select **raw → JSON** and enter the order data:

```json
{
  "orderId": "ORD003",
  "tableNumber": "05",
  "items": [
    {
      "name": "Cơm chiên",
      "price": 50000,
      "quantity": 2
    }
  ],
  "totalAmount": 100000
}
```

Click **Send** to submit the request.

Amazon API Gateway forwards the request to the `createOrder` Lambda Function. The Lambda Function processes the request and stores the new order in the `CloudMenuOrders` Amazon DynamoDB table.

The API returns the following HTTP status:

```text
200 OK
```

This result confirms that the order creation API is working successfully.

![Test POST API](/images/5-Workshop/5.5/test-post-order.png)

*Figure 1. Testing the POST /order API using Postman.*

---

### 2. Testing GET /orders

The `GET /orders` API is used to retrieve the list of orders stored in the system.

In Postman, select the **GET** method and enter the endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders
```

A GET request does not require a Request Body.

Click **Send** to submit the request.

Amazon API Gateway forwards the request to the `getOrders` Lambda Function. The Lambda Function retrieves the order data from the `CloudMenuOrders` DynamoDB table and returns the order list to the client.

The API returns:

```text
200 OK
```

The response contains information about the orders currently stored in the system.

![Test GET API](/images/5-Workshop/5.5/test-get-orders.png)

*Figure 2. Testing the GET /orders API using Postman.*

---

### 3. Testing PUT /orders/{orderId}

The `PUT /orders/{orderId}` API is used to update the status of an existing order.

In this test, the order with the `orderId` value `ORD003` is used.

In Postman, select the **PUT** method and enter the endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders/ORD003
```

In the **Body** section, select **raw → JSON** and enter:

```json
{
  "status": "PREPARING"
}
```

Click **Send** to submit the request.

Amazon API Gateway extracts the `ORD003` value from the `{orderId}` Path Parameter and forwards the request to the `updateOrderStatus` Lambda Function.

The Lambda Function identifies the corresponding order in the `CloudMenuOrders` DynamoDB table and updates its status.

The API returns:

```text
200 OK
```

This result confirms that the order status has been updated successfully.

![Test PUT API](/images/5-Workshop/5.5/test-put-order.png)

*Figure 3. Testing the PUT /orders/{orderId} API using Postman.*

---

### Testing Results

All three APIs return the HTTP status `200 OK`, confirming that the integration between Amazon API Gateway, AWS Lambda, and Amazon DynamoDB is working successfully.

The request flow can be summarized as follows:

```text
Postman
   ↓
Amazon API Gateway
   ↓
AWS Lambda
   ↓
Amazon DynamoDB
   ↓
Response
```

The API testing results are summarized below:

| API | Lambda Function | Result |
| :--- | :--- | :---: |
| `POST /order` | `createOrder` | `200 OK` |
| `GET /orders` | `getOrders` | `200 OK` |
| `PUT /orders/{orderId}` | `updateOrderStatus` | `200 OK` |

The APIs now support the main backend functions of CloudMenu:

- Creating a new order.
- Retrieving the order list.
- Updating the status of an existing order.

After verifying that all APIs work correctly, the CloudMenu backend is ready to be integrated with the frontend application.