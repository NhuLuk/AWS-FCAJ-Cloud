---

title: "Proposal"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

# CloudMenu – Table Ordering System on AWS

## AWS Serverless Solution for Ordering, Order Processing, and Status Tracking

### 1. Executive Summary

CloudMenu is a table-side online ordering system that allows customers to use their smartphones to scan a QR Code assigned to each table, access the menu, select dishes, and send orders directly to the kitchen.

The system is designed for small and medium-sized restaurants or food service businesses and includes three main user groups:

* **Customers**: browse the menu, search or filter dishes, manage the cart, place orders, and track order status.
* **Kitchen Staff**: receive orders, view order information, and update preparation status.
* **Admin/Manager**: monitor a dashboard generated from order data, including the number of orders, revenue, order status, revenue by table, and the most frequently ordered dishes.

CloudMenu is proposed to be deployed using an **AWS Serverless architecture**, with Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, and Amazon DynamoDB as the main components. This architecture reduces server management requirements, supports scaling based on traffic, and is suitable for restaurant workloads where the number of users may vary significantly throughout the day.

The objective of the project is to build an end-to-end system that can be deployed on AWS, covering frontend hosting, backend processing, database storage, APIs, basic security, testing, monitoring, and resource clean-up.

### 2. Problem Statement

*Current Problem*

In a traditional restaurant ordering process, customers usually need to wait for staff to come to the table and take their orders. During busy periods, this process can increase waiting time and place additional workload on restaurant staff.

Some common limitations include:

* Customers have to wait for staff to take their orders.
* Errors may occur when recording dishes, quantities, or table numbers.
* Kitchen staff may find it difficult to centrally track orders waiting to be processed.
* Order status updates between the kitchen and customers are not automated.
* Managers may have difficulty quickly summarizing order volume, revenue, and overall business activity.
* The system needs to handle increased traffic during peak hours without requiring continuously running servers.

*Solution*

CloudMenu uses a dedicated QR Code for each table so that customers can directly access the ordering interface. Table information is passed to the system through the QR Code, allowing customers to select dishes and submit their orders.

The main processing flow is:

**QR Code → Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

The CloudMenu frontend is stored in Amazon S3 and distributed through Amazon CloudFront. When a customer submits an order, the frontend calls a REST API provided by Amazon API Gateway. API Gateway forwards the request to AWS Lambda, which processes the business logic and stores order data in Amazon DynamoDB.

Kitchen staff use a separate interface to retrieve orders and update their statuses. Admins or managers use a dashboard to summarize and monitor order data.

The solution helps to:

* Automatically identify tables through QR Codes.
* Reduce manual order-taking operations.
* Minimize errors related to dishes, quantities, and table numbers.
* Centralize order data in Amazon DynamoDB.
* Allow kitchen staff to monitor and update order status.
* Provide a dashboard for monitoring restaurant operations.
* Take advantage of the automatic scalability of Serverless services.
* Reduce operational costs in development and low-traffic environments.

*Benefits and Solution Value*

CloudMenu helps transform the traditional restaurant ordering process into a simpler digital workflow. Customers can place orders without waiting for staff to manually record them, while kitchen staff have a centralized interface for managing incoming orders.

For managers, centralized order data makes it easier to monitor the number of orders, revenue, active tables, and frequently ordered dishes.

Using a Serverless architecture also means that the system does not require continuously running application servers. Services such as AWS Lambda and Amazon DynamoDB can scale according to actual usage, making the architecture suitable for an individual project, development environment, and workloads with variable traffic.

### 3. Solution Architecture

CloudMenu uses a Serverless architecture to deploy its frontend, backend, and data storage components on AWS.

Users access the frontend through Amazon CloudFront. CloudFront distributes the HTML, CSS, and JavaScript files stored in Amazon S3. The frontend sends requests to Amazon API Gateway, which forwards them to AWS Lambda for business logic processing. Lambda reads from or writes order data to Amazon DynamoDB.

AWS IAM provides IAM Roles that allow Lambda functions to access DynamoDB according to the **principle of least privilege**, avoiding the need to hard-code AWS Access Keys or Secret Access Keys in the source code.

Amazon CloudWatch is used to collect backend logs and metrics, helping monitor Lambda activity, detect errors, and support system testing and troubleshooting.

![AWS CloudMenu Architecture](/images/2-Proposal/AWS_CloudMenu.png)

*AWS Services Used*

* *Amazon S3*: Stores the CloudMenu frontend and static assets.
* *Amazon CloudFront*: Distributes frontend content through a CDN and HTTPS.
* *Amazon API Gateway*: Provides REST APIs for communication between the frontend and backend.
* *AWS Lambda*: Processes order creation, retrieves order lists, and updates order status.
* *Amazon DynamoDB*: Stores order data, table numbers, ordered items, and processing status.
* *AWS IAM*: Provides IAM Roles and controls permissions between Lambda and other AWS services according to the principle of least privilege.
* *Amazon CloudWatch*: Collects logs and metrics for monitoring, troubleshooting, and backend testing.
* *AWS Budgets*: Helps monitor AWS spending and provides alerts when costs exceed predefined thresholds.

*Component Design*

* *Customer Interface*: Allows customers to access the menu through a QR Code, identify their table, select dishes, manage the cart, place orders, and track order status.
* *Kitchen Interface*: Allows kitchen staff to view orders and update preparation status.
* *Admin Dashboard*: Summarizes order data to display order count, revenue, order status, revenue by table, and frequently ordered dishes.
* *Frontend Hosting*: Amazon S3 stores the frontend, while Amazon CloudFront provides HTTPS delivery and content distribution.
* *API Layer*: Amazon API Gateway receives requests from the frontend and routes them to the appropriate Lambda function.
* *Backend Processing*: AWS Lambda performs the application business logic.
* *Data Storage*: Amazon DynamoDB stores order data.
* *Security*: AWS IAM restricts permissions between Lambda and DynamoDB.
* *Monitoring*: Amazon CloudWatch stores logs and metrics and supports backend alerting.

### 4. Technical Implementation

*Implementation Phases*

The CloudMenu project is implemented through several phases, beginning with learning and architecture design and continuing through development, deployment, testing, monitoring, and clean-up:

1. *Research and Architecture Design*: Study AWS Cloud, Serverless Architecture, and AWS services suitable for CloudMenu.
2. *Data and Backend Design*: Create the Amazon DynamoDB table, IAM Role, and AWS Lambda functions.
3. *REST API Development*: Use Amazon API Gateway to connect the frontend to the Lambda backend.
4. *CloudMenu Interface Development*: Build the Customer Interface, Kitchen Interface, and Admin Dashboard.
5. *Frontend Deployment*: Upload the frontend to Amazon S3 and distribute it through Amazon CloudFront.
6. *End-to-End Integration*: Connect the frontend, API Gateway, Lambda, and DynamoDB into a complete system.
7. *Testing and Monitoring*: Test user workflows, inspect CloudWatch Logs and metrics, and troubleshoot errors.
8. *Clean-up and Documentation*: Remove unnecessary resources to prevent additional charges and complete the bilingual Workshop documentation.

*Technical Requirements*

* *AWS Account*: An AWS account with sufficient permissions to create and manage the resources used by the project.
* *AWS Region*: Backend resources are deployed in the same appropriate Region to simplify management.
* *Frontend*: HTML, CSS, and JavaScript.
* *Backend*: AWS Lambda using Python and the AWS SDK for Python (`boto3`).
* *Database*: Amazon DynamoDB with `orderId` as the Partition Key for the `CloudMenuOrders` table.
* *API*: Amazon API Gateway REST API.
* *Security*: IAM Role for Lambda and no hard-coded AWS credentials in the source code.
* *Monitoring*: Amazon CloudWatch Logs, Lambda metrics, and CloudWatch Alarm.
* *Development Tools*: Visual Studio Code, a web browser, and API testing tools when required.

### 5. Roadmap & Milestones

* *Week 1 (22/06 - 26/06) — AWS Cloud and Serverless Fundamentals*

  * Become familiar with the AWS Cloud platform.
  * Learn the fundamentals of Serverless Architecture.
  * Study Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, and AWS IAM.

* *Week 2 (29/06 - 03/07) — IAM, Amazon S3, and Amazon CloudFront*

  * Learn AWS IAM and the principle of least privilege.
  * Practice hosting a static website using Amazon S3.
  * Learn and deploy Amazon CloudFront.

* *Week 3 (06/07 - 10/07) — Amazon DynamoDB and Data Design*

  * Learn Amazon DynamoDB and the NoSQL data model.
  * Create the `CloudMenuOrders` table.
  * Define `orderId` as the Partition Key.
  * Design the order data structure.

* *Week 4 (13/07 - 17/07) — AWS Lambda and Serverless Backend*

  * Learn AWS Lambda and Function as a Service.
  * Create an IAM Role for Lambda.
  * Connect Lambda to DynamoDB using `boto3`.
  * Develop Lambda functions for order processing.

* *Week 5 (20/07 - 24/07) — Amazon API Gateway and REST API*

  * Build the REST API.
  * Integrate API Gateway with Lambda.
  * Configure endpoints used by CloudMenu.
  * Configure CORS and test the API.

* *Week 6 (27/07 - 31/07) — CloudMenu Analysis and Development*

  * Finalize functional requirements.
  * Build the QR Code-based table identification mechanism.
  * Develop the Customer Interface.
  * Develop the Kitchen Interface.

* *Week 7 (03/08 - 07/08) — Deployment and Integration on AWS*

  * Complete the main system components.
  * Upload the frontend to Amazon S3.
  * Distribute the frontend through Amazon CloudFront.
  * Connect the frontend with API Gateway, Lambda, and DynamoDB.
  * Test the Customer → Kitchen workflow.

* *Week 8 (10/08 - 15/08) — Completion, Monitoring, and Finalization*

  * Complete the Admin Dashboard.
  * Complete order time and status tracking.
  * Test the Customer, Kitchen, and Admin interfaces.
  * Inspect CloudWatch Logs and metrics.
  * Configure a CloudWatch Alarm for the backend.
  * Complete the architecture diagram, Workshop, README, and project documentation.
  * Clean up unused AWS resources.

### 6. Budget Estimation

CloudMenu uses AWS Serverless services and prioritizes AWS Free Tier where applicable during development and testing.

Actual costs depend on the number of requests, storage usage, data transfer, and CloudWatch log retention.

*Estimated Infrastructure Costs*

* Amazon S3: approximately 0–3 USD/month for frontend hosting and static assets.
* Amazon CloudFront: approximately 0–15 USD/month depending on data transfer and request volume.
* Amazon API Gateway: approximately 0–10 USD/month depending on the number of API requests.
* AWS Lambda: approximately 0–8 USD/month depending on invocation count and execution duration.
* Amazon DynamoDB: approximately 0–10 USD/month depending on request volume and stored data.
* Amazon CloudWatch: cost depends on log ingestion, metrics, and alarms.
* AWS IAM: no direct AWS charge for IAM Users, Roles, and Policies.
* AWS Budgets: used to monitor project spending and provide cost alerts.

*Estimated Total*: approximately **0–50 USD/month**, depending on traffic and resource consumption. In a low-traffic development environment where applicable Free Tier allowances remain available, the actual cost may be significantly lower.

*Cost Control Measures*

* Use AWS Free Tier where applicable.
* Configure AWS Budgets to notify when costs exceed expected limits.
* Monitor CloudWatch Logs and remove unnecessary retained logs.
* Optimize Amazon S3 storage usage.
* Review and delete unused AWS development resources.
* Prefer Serverless services to avoid continuously running servers.
* Perform a complete resource clean-up after finishing the Workshop.

### 7. Risk Assessment

*Risk Matrix*

* Unexpected AWS cost increase: Medium impact, low probability.
* Sudden traffic increase: Medium impact, medium probability.
* Lambda or API errors: High impact, medium probability.
* Accidental DynamoDB data loss or deletion: High impact, low probability.
* QR Code used for the wrong table: Medium impact, medium probability.
* Duplicate order submission: Medium impact, medium probability.
* Unintended API access: High impact, medium probability.

*Mitigation Strategy*

* *Cost*: Use AWS Budgets, monitor AWS Cost Management, and clean up unnecessary resources.
* *Traffic*: Take advantage of the scalability of API Gateway, Lambda, and DynamoDB.
* *Backend Errors*: Use Amazon CloudWatch Logs, metrics, and alarms to detect problems.
* *Data*: Use DynamoDB Point-in-Time Recovery or backups when appropriate.
* *QR Code*: Associate each QR Code with a table identifier and validate table information before creating an order.
* *Duplicate Requests*: Implement mechanisms to prevent repeated submissions and validate `orderId`.
* *API Access*: Configure CORS appropriately, restrict IAM permissions between AWS services, and consider adding authentication and authorization for administrative functions.

*Contingency Plan*

* Inspect CloudWatch Logs to identify the cause of backend failures.
* Test Lambda functions directly if API Gateway experiences issues during troubleshooting.
* Restore data from DynamoDB backup or Point-in-Time Recovery when enabled.
* Temporarily disable or delete unnecessary AWS resources if unexpected costs occur.
* Maintain source code and configuration documentation so that the system can be redeployed when necessary.

### 8. Expected Outcomes

*Technical Improvement*: CloudMenu provides an end-to-end ordering system on AWS where customers can place orders using QR Codes, kitchen staff can receive and update order status, and Admin/Managers can monitor operations through a dashboard.

*Deployment Outcome*: The frontend is delivered through Amazon CloudFront and Amazon S3; the backend is implemented using Amazon API Gateway, AWS Lambda, and Amazon DynamoDB; IAM controls permissions between AWS services; and Amazon CloudWatch supports logging and monitoring.

*Scalability*: The Serverless architecture allows the system to handle changes in traffic without requiring the management of fixed application servers.

*Long-Term Value*: CloudMenu can be extended in the future with employee authentication, dynamic menu management, online payments, real-time notifications, table reservations, and more advanced analytics capabilities.
