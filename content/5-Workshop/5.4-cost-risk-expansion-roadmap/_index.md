---

title : "Cost, Risks, and Future Expansion"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4. </b> "
-----------------------

## 5.4. Cost, Risks, and Future Expansion

### Cost Optimization

CloudMenu is built using a serverless architecture and managed AWS services to reduce operational costs and avoid maintaining continuously running servers. The main services that need to be monitored in terms of cost include Amazon S3, Amazon CloudFront, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, and Amazon CloudWatch.

For development and testing environments, several cost optimization measures can be applied:

* Use Amazon S3 to store frontend files while monitoring storage usage, request volume, and data transfer.
* Monitor Amazon CloudFront requests and data transfer, especially when the number of users increases.
* Optimize the number of AWS Lambda invocations and execution duration to reduce costs related to invocation and compute time.
* Design DynamoDB according to the actual CloudMenu workload, reduce unnecessary read operations, and optimize access patterns for the `CloudMenuOrders` table.
* Monitor the number of requests sent to Amazon API Gateway, particularly for frequently called APIs such as Get Orders.
* Manage CloudWatch Logs appropriately and configure suitable log retention periods to avoid storing unnecessary log data for long periods.
* Take advantage of AWS Free Tier where appropriate for the testing environment and regularly review costs through AWS Billing and Cost Explorer.
* Only introduce additional services such as VPC, NAT Gateway, AWS WAF, or Amazon Cognito when the system has an actual requirement, helping keep the architecture simple and avoid unnecessary costs.

Because CloudMenu uses Lambda and DynamoDB in a serverless model, operational costs can increase or decrease according to actual usage instead of requiring a fixed cost for a continuously running application server.

### Risks and Mitigation

CloudMenu has several potential risks that should be considered during development and deployment.

**Cost increase risk:** A significant increase in requests to API Gateway, Lambda, DynamoDB, or CloudFront may result in higher usage-based costs.

**Configuration risk:** Incorrect configuration of Amazon S3, IAM policies, API Gateway, or CloudFront may cause unauthorized access or prevent the system from operating correctly.

**Data loss or inconsistency risk:** Errors during order creation, order retrieval, or status updates may result in incorrect information being stored in the `CloudMenuOrders` table.

**Credential exposure risk:** Storing AWS Access Keys, Secret Access Keys, or other sensitive information directly in source code may lead to credential leakage.

**Manual frontend deployment risk:** CloudMenu currently uploads frontend files to Amazon S3 manually. As a result, files may be missed, an incorrect version may be uploaded, or CloudFront may continue to distribute an older version of the application.

**Lambda update risk:** Changes to backend logic or API Gateway configuration may affect the Customer, Kitchen, and Manager interfaces.

**AWS service dependency risk:** CloudMenu depends on several AWS managed services. Therefore, future architecture changes need to consider the dependencies and compatibility between these services.

The following measures can be used to reduce these risks:

* Apply the **Least Privilege** principle to IAM Roles and policies.
* Do not store AWS credentials or secrets directly in source code or commit them to GitHub.
* Configure Amazon S3 permissions appropriately and restrict write or delete permissions when they are not required.
* Do not allow the frontend to access DynamoDB directly; all data operations should go through API Gateway and Lambda.
* Use CloudWatch Logs and Metrics to identify errors and monitor unusual system activity.
* Establish appropriate backup and recovery mechanisms for DynamoDB data.
* Test important APIs before deploying a new version.
* Use a deployment checklist when uploading frontend files or updating Lambda functions to reduce errors during manual deployment.

### Future Expansion Roadmap

CloudMenu can be expanded gradually according to the scale and actual requirements of the system.

* **CI/CD automation:** Build an automated pipeline from GitHub to build and deploy the frontend to Amazon S3 while also automating the deployment of Lambda functions.
* **Improved testing:** Add unit tests, integration tests, and smoke tests for important workflows such as order creation, order retrieval, and order-status updates.
* **Authentication and authorization:** Integrate Amazon Cognito when the system requires account management and role-based access for Customer, Kitchen, and Manager users.
* **Improved API security:** Add authentication, authorization, rate limiting, and AWS WAF when CloudMenu is deployed in a production environment with higher traffic.
* **Improved observability:** Build CloudWatch Dashboards and Alarms to monitor API Gateway, Lambda, DynamoDB, and detect errors or unusual resource usage.
* **Data model expansion:** Add tables or entities such as `MenuItems`, `Tables`, `Restaurants`, and `OrderHistory` when the system requires more complete restaurant-management functionality.
* **Multi-restaurant support:** If CloudMenu evolves into a multi-tenant platform, the data model and authorization mechanism can be expanded so that each restaurant can independently manage its menu, tables, and orders.
* **Asynchronous processing:** When background tasks such as sending notifications, updating reports, or synchronizing data are required, suitable AWS event-driven services can be introduced.
* **Performance optimization:** As request volume and data size increase, caching, DynamoDB Streams, or suitable analytics services can be considered according to the actual workload.
* **Improved network security:** VPC, VPC Endpoints, or more complex network components should only be added when the backend needs access to private resources or requires stronger network-level controls.

The objective of this roadmap is to keep CloudMenu simple, serverless, and cost-efficient during the development stage while providing a foundation for future growth as the number of restaurants, users, and orders increases.
