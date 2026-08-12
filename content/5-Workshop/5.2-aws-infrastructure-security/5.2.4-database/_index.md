---

title : "Database"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.2.4 </b> "
------------------------

## 5.2.4. Database

### Overview

CloudMenu uses **Amazon DynamoDB** as its primary database for storing and managing order data. DynamoDB is a fully managed NoSQL database service provided by AWS and is well suited to the serverless backend architecture implemented for CloudMenu.

In the current architecture, the frontend does not access the database directly. Requests from Customer, Kitchen, or Manager interfaces are sent to Amazon API Gateway and then forwarded to AWS Lambda for processing. Lambda performs the required read or write operations on DynamoDB and returns the result to the frontend through API Gateway.

The primary data access flow can be summarized as follows:

**Customer/Kitchen/Manager → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Using DynamoDB allows CloudMenu to operate without deploying and maintaining a dedicated database server. Compared with an architecture based on a relational database deployed on Amazon RDS, the current solution does not require CloudMenu to maintain a database instance, manage application database connections, or introduce additional network components solely to provide database connectivity.

### CloudMenuOrders Data Design

Within the current scope of the Workshop, order data is stored in the following table:

**`CloudMenuOrders`**

Each order is identified by a unique `orderId`. The `orderId` attribute is used as the **Partition Key**, allowing individual orders to be uniquely identified and accessed.

An item in the table may contain the following primary attributes:

| Attribute       | Description                                                       |
| :-------------- | :---------------------------------------------------------------- |
| **orderId**     | Unique identifier of the order and the Partition Key of the table |
| **tableNumber** | Number of the table from which the order was submitted            |
| **items**       | List of menu items and their corresponding quantities             |
| **totalAmount** | Total value of the order                                          |
| **status**      | Current processing status of the order                            |
| **createdAt**   | Timestamp indicating when the order was created                   |
| **updatedAt**   | Timestamp of the most recent order update                         |

This structure supports the primary CloudMenu operations implemented in the current phase, including creating new orders, retrieving order information, and updating order statuses.

When a Customer finishes selecting menu items and submits an order, the frontend sends the order information to API Gateway. Lambda processes the request and creates the corresponding item in the `CloudMenuOrders` table.

When the Kitchen interface needs to display pending or processing orders, the backend retrieves the required data from DynamoDB and returns it to the Kitchen interface. When kitchen staff change the status of an order, Lambda updates the corresponding `status` attribute and its update timestamp.

The Manager Dashboard also uses order information retrieved by the backend from DynamoDB to calculate and display information such as order counts, revenue, order statuses, table-related activity, and frequently ordered menu items.

### Why Amazon DynamoDB Was Selected

DynamoDB was selected because it aligns well with the serverless architecture used by the CloudMenu backend.

The main reasons include:

* No dedicated database server needs to be deployed or maintained.
* Direct integration with AWS Lambda through the AWS SDK.
* Suitable for the relatively straightforward order data used in the current project scope.
* Capable of handling changing workloads without requiring management of a fixed database instance.
* Reduces the number of infrastructure components that need to be configured and maintained.
* Suitable for development and testing environments and the Workshop's cost-optimization objectives.

Using DynamoDB also keeps the backend architecture consistent with CloudMenu's serverless approach. API Gateway, Lambda, and DynamoDB can work together without requiring a continuously running traditional application server or database server.

### Data Read and Write Flow

CloudMenu database operations are performed through Lambda functions rather than directly from the frontend.

For order creation, the primary flow is:

**Customer → API Gateway → Lambda → Put item into `CloudMenuOrders`**

For retrieving order information:

**Kitchen/Manager → API Gateway → Lambda → Read `CloudMenuOrders` → Return data to frontend**

For updating an order status:

**Kitchen → API Gateway → Lambda → Update order in `CloudMenuOrders`**

This approach separates the user interface from the data storage layer. If the data-processing logic needs to change, most modifications can be implemented in the backend without allowing the frontend to directly access or depend on DynamoDB configuration details.

### Data Security

CloudMenu does not provide direct browser access to Amazon DynamoDB.

The frontend only sends requests to API endpoints exposed through Amazon API Gateway. Lambda functions are responsible for performing the required database operations after receiving requests from API Gateway.

Access from Lambda to DynamoDB is controlled through an **AWS IAM Role**. Lambda is granted only the permissions required to perform CloudMenu operations on the `CloudMenuOrders` table.

The access architecture can therefore be summarized as:

**Frontend → API Gateway → Lambda → IAM-authorized access → DynamoDB**

This approach avoids storing AWS Access Keys or Secret Access Keys in frontend JavaScript and supports the **Least Privilege** principle for access to AWS resources.

Lambda execution activity is also recorded through Amazon CloudWatch Logs. These logs support request inspection, backend error identification, and troubleshooting when DynamoDB operations do not complete successfully.

### Scope of the Current Design

In the current implementation, `CloudMenuOrders` focuses on the data required for the core table-ordering workflow. This design is appropriate for the scope of the Workshop but should not be considered a complete data model for a production-scale restaurant management platform.

For example, menu information does not necessarily use a complete dynamic data-management system in the current implementation, and CloudMenu does not currently implement a multi-tenant data model for multiple restaurants.

Keeping the current scope limited allows the project to focus on the primary business flow:

**Customer creates order → Backend stores data → Kitchen processes order → Manager monitors data**

### Future Database Expansion

As CloudMenu evolves, the data model can be expanded to support additional entities and business requirements, such as:

* **Menu Items:** menu item names, prices, categories, images, and availability.
* **Tables:** restaurant table information and the corresponding QR Codes.
* **Orders:** additional order information and attributes required for more advanced processing.
* **Restaurants:** support for multiple restaurants if CloudMenu evolves into a multi-tenant platform.
* **Order History:** historical data used for reporting and operational analysis.

Depending on future requirements, CloudMenu could also evaluate **DynamoDB Streams** for event-driven processing, introduce caching mechanisms when read traffic increases, or integrate AWS analytics services for more advanced reporting and data analysis.

Future expansion should be driven by actual application access patterns rather than simply adding tables or attributes. This is particularly important when working with DynamoDB because NoSQL data models should be designed around how the application needs to access its data.

For the current CloudMenu implementation, the `CloudMenuOrders` table provides the data foundation required by the Customer Interface, Kitchen Interface, and Manager Dashboard while keeping the database architecture simple and aligned with the serverless design of the system.
