---
title: "System Testing"
date: 2026-06-22
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---

## 5.8. System Testing

After completing the deployment of the CloudMenu frontend and backend, the system is tested directly through the user interfaces to verify that the components can work together in an actual application workflow.

Individual API testing was already performed in **Section 5.5.4. API Testing**. Therefore, this section focuses on functional testing through the user interfaces and the End-to-End workflow of CloudMenu.

The main interfaces tested include:

- Customer ordering interface.
- Order tracking interface.
- Kitchen order management interface.
- Restaurant Dashboard.

The overall testing flow is:

```text
Customer
   |
   v
View Menu & Create Order
   |
   v
Order Tracking
   |
   v
Kitchen / Restaurant Orders
   |
   v
Update Order Status
   |
   v
Restaurant Dashboard
```

---

### 5.8.1. Customer Ordering Interface Testing

First, the CloudMenu Customer Interface is accessed to test the menu browsing and ordering process.

The interface displays the available dishes together with the main information and functions, including:

- Dish images.
- Dish names.
- Prices.
- Food categories.
- Search and filtering functions.
- Add-to-cart functionality.

![CloudMenu Customer Menu](/images/5-Workshop/5.8/customer-menu.png)

The customer selects dishes and adds them to the shopping cart. After reviewing the selected items and the total order amount, the customer submits the order.

When the order is submitted, the frontend sends the order information to the backend through Amazon API Gateway.

The processing flow is:

```text
Customer Interface
        |
        v
POST /order
        |
        v
Amazon API Gateway
        |
        v
createOrder Lambda
        |
        v
Amazon DynamoDB
```

After the order is successfully created, the order information is stored in the `CloudMenuOrders` table with the initial status:

```text
PENDING
```

**Result:** The Customer Interface successfully displays the menu, allows customers to select dishes, and creates an order.

---

### 5.8.2. Order Tracking Testing

After successfully creating an order, the customer can access the order tracking interface to monitor its processing status.

![CloudMenu Order Tracking](/images/5-Workshop/5.8/order-tracking.png)

The tracking interface displays information such as:

- Order ID.
- Table number.
- Ordered items.
- Total order amount.
- Order creation time.
- Current order status.

During processing, the order status follows this workflow:

```text
PENDING
   |
   v
PREPARING
   |
   v
COMPLETED
```

The statuses represent:

- `PENDING`: The order has been created and is waiting for the kitchen to process it.
- `PREPARING`: The kitchen has accepted the order and is preparing the dishes.
- `COMPLETED`: The order preparation has been completed.

When the restaurant updates an order status, the new data is stored in Amazon DynamoDB and the corresponding status is displayed on the order tracking interface.

**Result:** Customers can successfully monitor the processing status of their orders after placing them.

---

### 5.8.3. Kitchen Order Management Testing

Next, the order management interface used by the kitchen staff is tested.

Orders created from the Customer Interface appear on this interface so that restaurant staff can monitor and process them.

![CloudMenu Restaurant Orders](/images/5-Workshop/5.8/restaurant-orders.png)

Each order displays information such as:

- Order ID.
- Table number.
- Order creation time.
- Ordered items.
- Total order amount.
- Current status.

When a new order is created, its initial status is:

```text
PENDING
```

When the kitchen staff starts processing the order, the status is updated to:

```text
PREPARING
```

After the dishes have been prepared, the status is updated to:

```text
COMPLETED
```

When the status is changed through the interface, the frontend sends the following request:

```text
PUT /orders/{orderId}
```

to the backend.

The processing flow is:

```text
Kitchen Interface
       |
       v
PUT /orders/{orderId}
       |
       v
Amazon API Gateway
       |
       v
updateOrderStatus Lambda
       |
       v
Amazon DynamoDB
```

After DynamoDB is updated, the new order status can be used by the related interfaces in the system.

**Result:** The kitchen order management interface successfully displays orders created from the Customer Interface and allows staff to update their processing status.

---

### 5.8.4. Restaurant Dashboard Testing

Finally, the Restaurant Dashboard is tested to verify the aggregation and display of CloudMenu operational data.

![CloudMenu Restaurant Dashboard](/images/5-Workshop/5.8/restaurant-dashboard.png)

The Dashboard displays summary information such as:

- Total number of orders.
- Total revenue.
- Number of pending orders.
- Number of orders being prepared.
- Number of completed orders.
- Revenue by table.
- Most frequently ordered dishes.

The information displayed on the Dashboard is aggregated from the order data currently available in the system.

When customers create new orders or restaurant staff update order statuses, the new information is stored in DynamoDB and used to update the statistics displayed on the Dashboard.

**Result:** The Restaurant Dashboard successfully displays aggregated order information and allows restaurant staff to monitor the overall operation of the system.

---

### 5.8.5. Testing Results

After testing the main CloudMenu interfaces, the following results were obtained:

| Function | Result |
| :--- | :--- |
| Display menu | Successful |
| Search and select dishes | Successful |
| Add dishes to cart | Successful |
| Create order | Successful |
| Track order status | Successful |
| Display orders on the kitchen interface | Successful |
| Update order status | Successful |
| Display dashboard statistics | Successful |

The CloudMenu End-to-End workflow was tested as follows:

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
PENDING
   |
   v
Kitchen receives order
   |
   v
PREPARING
   |
   v
COMPLETED
   |
   v
Restaurant Dashboard
```

The testing results show that the main CloudMenu interfaces can work together with the backend to complete the application workflow, from browsing the menu and creating an order to tracking its status and allowing restaurant staff to process and complete the order.

The underlying APIs were tested separately in **Section 5.5.4**, while the results in this section confirm that those APIs can be successfully used as part of the actual CloudMenu application workflow.