---
title: "Test API"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.5.4. </b>"
---

## 5.5.4. Test API

After configuring the Routes and Lambda Integrations, the APIs are tested to verify that API Gateway correctly forwards requests to the appropriate Lambda Functions and that the system processes the requests successfully.

CloudMenu provides three main APIs that are tested using Postman:

| Method | Endpoint | Function |
|---|---|---|
| POST | `/order` | Create a new order |
| GET | `/orders` | Retrieve the order list |
| PUT | `/orders/{orderId}` | Update an order status |

---

### 1. Test POST /order

The `POST /order` API is used to create a new order.

In Postman, select the **POST** method and enter the endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/order
```

In the **Body** section, select **raw → JSON** and enter the order information:

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

API Gateway forwards the request to the `createOrder` Lambda Function. The Lambda Function processes the request and stores the order in the `CloudMenuOrders` DynamoDB table.

The API returns:

```text
200 OK
```

This confirms that the order creation API is working successfully.

![Test POST API](images/test-post-order.png)

---

### 2. Test GET /orders

The `GET /orders` API is used to retrieve the list of orders stored in the system.

In Postman, select the **GET** method and enter the endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders
```

API Gateway forwards the request to the `getOrders` Lambda Function. The Lambda Function retrieves the order data from the `CloudMenuOrders` DynamoDB table and returns the result to the client.

No request body is required for the GET request.

Click **Send** to submit the request.

The API returns:

```text
200 OK
```

The response contains the orders currently stored in the system.

![Test GET API](images/test-get-orders.png)

---

### 3. Test PUT /orders/{orderId}

The `PUT /orders/{orderId}` API is used to update the status of an existing order.

In this test, the order with the `orderId` `ORD003` is updated.

Select the **PUT** method and use the following endpoint:

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

API Gateway extracts `ORD003` from the `{orderId}` path parameter and forwards the request to the `updateOrderStatus` Lambda Function.

The Lambda Function updates the corresponding order in the `CloudMenuOrders` DynamoDB table.

The API returns:

```text
200 OK
```

This confirms that the order status was updated successfully.

![Test PUT API](images/test-put-order.png)

---

### Test Results

All three APIs returned an HTTP `200 OK` response, confirming that the integration between API Gateway, AWS Lambda, and Amazon DynamoDB is working successfully.

The request flow can be summarized as:

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

The APIs currently support the core CloudMenu operations:

- Create a new order.
- Retrieve the order list.
- Update an order status.

After verifying that all APIs work correctly, the backend is ready to be integrated with the CloudMenu user interface.