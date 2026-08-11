---
title: "System Testing"
date: 2026-06-22
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---

## 5.8. System Testing

After deploying the main components of CloudMenu, the system was tested to verify that the frontend, APIs, and database could operate together successfully.

The testing process focused on two main areas:

- **API Testing:** Verifying that the APIs for creating orders, retrieving orders, and updating order status work correctly.
- **Functional Testing:** Verifying that the actual CloudMenu interfaces operate correctly for customers and restaurant staff.

The overall system flow tested is:

```text
Customer
   |
   v
CloudMenu Frontend
   |
   v
Amazon API Gateway
   |
   v
AWS Lambda
   |
   v
Amazon DynamoDB
   |
   +-----------------------+
   |                       |
   v                       v
Order Tracking      Restaurant Management
                            |
                            v
                    Restaurant Dashboard
```

### API Testing

#### Create an Order

The order creation API is:

```text
POST /order
```

Endpoint:

```text
https://zcbix27iq9.execute-api.us-east-1.amazonaws.com/order
```

The following Request Body was used during testing:

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

Amazon API Gateway forwards the request to the `createOrder` Lambda Function, which processes the order and stores the data in Amazon DynamoDB.

The test returned:

```text
200 OK
```

![Test POST Order](images/test-post-order.png)

The result confirms that the order creation API can successfully receive and process the request.

---

#### Retrieve the Order List

The API is:

```text
GET /orders
```

Endpoint:

```text
https://zcbix27iq9.execute-api.us-east-1.amazonaws.com/orders
```

The request is forwarded to the `getOrders` Lambda Function, which retrieves order data from the `CloudMenuOrders` table.

The test returned:

```text
200 OK
```

![Test GET Orders](images/test-get-orders.png)

The result confirms that the order list can be successfully retrieved through Amazon API Gateway.

---

#### Update an Order Status

The API is:

```text
PUT /orders/{orderId}
```

During testing, the `ORD003` order was used:

```text
PUT /orders/ORD003
```

Request Body:

```json
{
  "status": "PREPARING"
}
```

The request is forwarded to the `updateOrderStatus` Lambda Function.

The function retrieves the `orderId` from the path parameter and updates the corresponding order status in DynamoDB.

The test returned:

```text
200 OK
```

![Test PUT Order](images/test-put-order.png)

The result confirms that the order status can be successfully updated.

---

### Error Handling During Testing

During the first test of the order status update API, the system returned:

```text
500 Internal Server Error
```

with the following message:

```text
Object of type Decimal is not JSON serializable
```

The issue occurred because numeric values retrieved from DynamoDB can be represented using the Python `Decimal` type, while Python's `json.dumps()` does not automatically serialize this type.

The `updateOrderStatus` Lambda Function was updated to convert DynamoDB `Decimal` values into JSON-serializable values before generating the HTTP response.

After updating the Lambda Function and testing:

```text
PUT /orders/ORD003
```

again, the API returned:

```text
200 OK
```

This confirmed that the serialization issue had been resolved successfully.

---

### CloudMenu Functional Testing

After confirming that the backend APIs worked correctly, the CloudMenu interfaces were tested to verify the actual application workflow.

#### Customer Menu

The customer menu interface allows users to browse and select available dishes.

The main information and functions include:

- Dish images.
- Dish names.
- Prices.
- Categories.
- Add-to-cart functionality.
- Search and filtering features.

![CloudMenu Customer Menu](images/customer-menu.png)

The test confirms that the menu interface can display dishes and provide the functions required for customers to begin the ordering process.

---

#### Order Tracking

After an order is created, customers can monitor its processing status.

![CloudMenu Order Tracking](images/order-tracking.png)

The order tracking interface displays information including:

- Order ID.
- Order time.
- Estimated preparation time.
- Current order status.
- Order details.
- Total order amount.

The order status flow is represented as:

```text
Order Submitted
      |
      v
   Preparing
      |
      v
   Completed
```

This feature allows customers to monitor the progress of their orders after submission.

---

#### Restaurant Order Management

Restaurant staff use the order management interface to monitor and process incoming customer orders.

![CloudMenu Restaurant Orders](images/restaurant-orders.png)

Each order displays information such as:

- Order ID.
- Table number.
- Creation time.
- Ordered items.
- Total amount.
- Current status.

Restaurant staff can update the order status according to the following workflow:

```text
PENDING
   |
   v
PREPARING
   |
   v
COMPLETED
```

Status updates are processed through the API:

```text
PUT /orders/{orderId}
```

When the status is changed through the interface, the request is sent to the backend and the corresponding data is updated in DynamoDB.

---

#### Restaurant Dashboard

CloudMenu provides a Dashboard that allows restaurant staff to monitor the overall activity of the system.

![CloudMenu Restaurant Dashboard](images/restaurant-dashboard.png)

The Dashboard displays summarized information including:

- Total number of orders.
- Total revenue.
- Pending orders.
- Orders being prepared.
- Completed orders.
- Revenue by table.
- Most frequently ordered dishes.

The Dashboard aggregates order data into useful indicators that support restaurant activity monitoring.

---

### End-to-End Testing

After testing the individual functions, the complete CloudMenu workflow was verified from the customer side to the restaurant side.

```text
Customer
   |
   v
View Menu
   |
   v
Create Order
   |
   v
Amazon API Gateway
   |
   v
createOrder Lambda
   |
   v
Amazon DynamoDB
   |
   +-------------------------+
   |                         |
   v                         v
Order Tracking        Restaurant Orders
                              |
                              v
                       Update Status
                              |
                              v
                    Amazon API Gateway
                              |
                              v
                  updateOrderStatus Lambda
                              |
                              v
                       Amazon DynamoDB
                              |
                              v
                   Restaurant Dashboard
```

The overall test results are summarized below:

| Function | Result |
| :--- | :--- |
| Display menu | Successful |
| Create order | 200 OK |
| Retrieve orders | 200 OK |
| Track order status | Successful |
| Update order status | 200 OK |
| Restaurant order management | Successful |
| Display Dashboard | Successful |

### Result

The main CloudMenu functions were successfully tested.

The backend APIs can create, retrieve, and update orders through Amazon API Gateway, AWS Lambda, and Amazon DynamoDB. The customer and restaurant interfaces also support menu browsing, ordering, order tracking, order processing, and dashboard monitoring.

The testing results demonstrate that the main components of CloudMenu can work together to support the application's complete workflow.