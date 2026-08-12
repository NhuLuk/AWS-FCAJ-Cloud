---

title : "System Overview and Architecture"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
-----------------------

## 5.1. System Overview and Architecture

### Overview

CloudMenu is a table-ordering web application designed and deployed on AWS. The system aims to digitalize the ordering process in restaurants and small food-service businesses by allowing customers to browse the menu, select items, and submit orders directly from their mobile devices through a QR code assigned to each table.

Instead of relying on printed menus and waiting for staff to manually record an order, each table is provided with a QR code containing its identification information. When a customer scans the QR code, the browser opens the CloudMenu Customer interface and passes the corresponding table information to the application. Customers can then browse available dishes, select quantities, manage their cart, submit an order, and track its processing status.

CloudMenu consists of three main interfaces designed for different roles in the restaurant workflow:

* **Customer Interface:** provides customers with access to the table-specific menu, item selection, cart management, order submission, and order status tracking.
* **Kitchen Interface:** allows kitchen staff to monitor incoming customer orders and update their processing status during preparation.
* **Manager Dashboard:** provides managers with an overview of orders, revenue, order statuses, table activity, and other relevant operational statistics.

Separating the application into role-specific interfaces allows each user group to focus on the functions relevant to its responsibilities while sharing the same backend services and order data source.

### System Architecture

CloudMenu is implemented using a serverless architecture on AWS to reduce the need for traditional server management and simplify operations within the scope of this Workshop.

The overall system architecture is illustrated below:

![AWS CloudMenu Architecture](/images/AWS_CloudMenu.png)

The CloudMenu architecture can be divided into three primary layers: frontend delivery, backend processing, and data storage.

The **frontend layer** uses Amazon S3 to store static application files such as HTML, CSS, JavaScript, and other user-interface assets. Amazon CloudFront distributes the content stored in S3 to users through a content delivery network. Customer, Kitchen, and Manager users access their respective interfaces through the frontend distributed by CloudFront.

The **backend layer** uses Amazon API Gateway as the entry point for HTTP requests from the frontend. API Gateway routes incoming requests to AWS Lambda, where the application business logic is executed. Lambda functions are responsible for core operations such as creating orders, retrieving order information, and updating order statuses.

The **data layer** uses Amazon DynamoDB to store CloudMenu order data. After a request is processed by Lambda, order information can be written to or retrieved from the `CloudMenuOrders` table. The result is then returned to the frontend through API Gateway.

The primary application data flow can be summarized as follows:

**Customer/Kitchen/Manager → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

For frontend content delivery, the access flow is:

**User → Amazon CloudFront → Amazon S3**

AWS IAM is used to control access between backend components. Lambda functions are assigned IAM Roles containing the permissions required to interact with DynamoDB instead of storing AWS Access Keys or Secret Access Keys directly in the source code. Restricting permissions according to operational requirements supports the **Least Privilege** principle.

In addition to the core processing components, Amazon CloudWatch collects backend logs and metrics to support system testing, operational monitoring, and troubleshooting when Lambda functions encounter errors.

### Architecture Characteristics

Using managed and serverless AWS services allows CloudMenu to operate without maintaining a continuously running backend server. API Gateway receives requests, Lambda executes business logic when required, and DynamoDB provides persistent data storage. This model is suitable for the current scope of CloudMenu, where traffic may vary over time and the system is being implemented at a development and testing scale.

The architecture also provides a clear separation of responsibilities between components. Amazon S3 and CloudFront handle frontend hosting and content delivery, API Gateway provides the API layer, Lambda performs business processing, DynamoDB stores application data, IAM controls service permissions, and CloudWatch supports monitoring and troubleshooting.

Within the current scope, CloudMenu focuses on the core functionality required for the table-ordering workflow and does not attempt to implement every component expected in a production-grade platform. In future development phases, the architecture can be extended with Amazon Cognito for employee authentication and authorization, AWS WAF for additional protection of public endpoints, CI/CD pipelines for automated deployment, and more advanced monitoring and alerting mechanisms.

This architecture provides the foundation for the following sections of the Workshop, which progressively implement the AWS infrastructure, serverless backend, database, API layer, frontend hosting, system testing, and monitoring capabilities of CloudMenu.
