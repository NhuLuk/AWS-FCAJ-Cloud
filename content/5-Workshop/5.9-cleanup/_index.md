---
title: "Resource Cleanup"
date: 2026-06-22
weight: 9
chapter: false
pre: "<b>5.9. </b>"
---

## 5.9. Resource Cleanup

After completing the deployment, testing, and evaluation of CloudMenu, AWS resources that are no longer required should be cleaned up to prevent unexpected costs and keep the AWS environment organized.

The main AWS services used in this Workshop include:

- Amazon CloudFront
- Amazon S3
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- AWS Identity and Access Management (IAM)

> **Note:** In the current Workshop environment, CloudMenu resources are still maintained for testing, demonstration, and evaluation purposes. The cleanup procedure below should be performed after the Workshop has been completed and the system is no longer required.

---

### 1. Clean Up Amazon CloudFront

Amazon CloudFront is used to distribute the CloudMenu frontend from Amazon S3 to users.

Navigate to:

```text
AWS Management Console
→ CloudFront
→ Distributions
```

Select the Distribution:

```text
CloudMenu
```

Before deleting the Distribution, verify that CloudMenu is no longer required for testing or demonstration.

Then disable the Distribution using the appropriate option provided in the AWS Management Console and wait for the update process to complete.

Once the Distribution has been disabled and is no longer serving requests, it can be deleted.

After the CloudFront Distribution is removed, the domain:

```text
d3be9t7i3323e7.cloudfront.net
```

will no longer be used to distribute the CloudMenu frontend.

Therefore, the system demonstration and evaluation should be completed before performing this step.

---

### 2. Clean Up Amazon S3

Amazon S3 is used to store the static frontend files of CloudMenu.

The bucket used in this Workshop is:

```text
ozmr-s3-demo-bucket
```

Navigate to:

```text
AWS Management Console
→ Amazon S3
→ Buckets
→ ozmr-s3-demo-bucket
```

Before deleting the bucket, review and remove the objects that are no longer required.

The frontend files and directories may include:

```text
frontend/
index.html
order.html
kitchen.html
dashboard.html
app.js
style.css
```

The actual list of files may vary depending on the frontend version currently deployed.

After confirming that the objects are no longer required, delete them from the bucket.

Next, return to the S3 bucket list and select:

```text
ozmr-s3-demo-bucket
```

After verifying that the bucket no longer contains data that needs to be retained, the bucket can be deleted.

---

### 3. Clean Up Amazon API Gateway

Amazon API Gateway is used to receive requests from the frontend and forward them to the corresponding AWS Lambda Functions.

Navigate to:

```text
AWS Management Console
→ API Gateway
→ APIs
```

Select:

```text
CloudMenuAPI
```

CloudMenuAPI provides the following main routes:

```text
POST /order
GET /orders
PUT /orders/{orderId}
```

Before deleting the API, verify that the CloudMenu frontend no longer needs to perform the following operations:

- Create new orders.
- Retrieve the order list.
- Update order status.

After confirming that the API is no longer required, delete:

```text
CloudMenuAPI
```

Once the API Gateway API is deleted, the CloudMenu API endpoints will no longer be available, and the frontend will no longer be able to send requests to the backend through these endpoints.

---

### 4. Clean Up AWS Lambda

AWS Lambda is used to process the backend logic of CloudMenu.

Navigate to:

```text
AWS Management Console
→ Lambda
→ Functions
```

The Lambda Functions used by CloudMenu include:

```text
createOrder
getOrders
updateOrderStatus
```

The Functions provide the following operations:

| Lambda Function | Functionality |
| :--- | :--- |
| `createOrder` | Creates a new order and stores it in DynamoDB. |
| `getOrders` | Retrieves the list of orders from DynamoDB. |
| `updateOrderStatus` | Updates the status of an existing order. |

After API Gateway has been cleaned up and the Functions are no longer required, delete the following Functions:

```text
createOrder
getOrders
updateOrderStatus
```

Before deleting them, verify that the Functions are not being used by API Gateway or any other AWS resources.

---

### 5. Clean Up Amazon DynamoDB

Amazon DynamoDB is used to store CloudMenu order data.

Navigate to:

```text
AWS Management Console
→ DynamoDB
→ Tables
```

Select the table:

```text
CloudMenuOrders
```

The table contains order-related information such as:

- `orderId`
- `tableNumber`
- `items`
- `totalAmount`
- `status`
- `createdAt`
- `updatedAt`
- `completedAt`

Before deleting the table, verify whether the order data is still required for testing, reporting, or system evaluation.

If the data is no longer required, delete the table:

```text
CloudMenuOrders
```

> **Note:** Deleting the `CloudMenuOrders` table will remove the order data stored in the table. If the data needs to be retained, it should be backed up or exported before deletion.

---

### 6. Review IAM Roles and Policies

AWS Lambda Functions require IAM Execution Roles to write logs to Amazon CloudWatch Logs and access the DynamoDB table.

For example, the `createOrder` Function uses the following Execution Role:

```text
createOrder-role-fmflntg9
```

This Role provides the permissions required by the Lambda Function, including logging permissions and access to the `CloudMenuOrders` table.

The DynamoDB-related Policy used during the CloudMenu deployment is:

```text
CloudMenuDynamoPolicy
```

After the Lambda Functions have been deleted, navigate to:

```text
AWS Management Console
→ IAM
→ Roles
```

Review the IAM Roles created specifically for CloudMenu.

Next, navigate to:

```text
AWS Management Console
→ IAM
→ Policies
```

Review the Policies that are no longer required.

If a Role or Policy is no longer attached to or used by any AWS resource, it can be removed.

> **Note:** Do not delete an IAM Role or Policy if it is still being used by another AWS resource. Dependencies should be verified before deletion.

---

### Recommended Cleanup Order

To minimize dependencies between resources, the cleanup process can be performed in the following order:

```text
Amazon CloudFront
        ↓
Amazon S3 Objects
        ↓
Amazon S3 Bucket
        ↓
Amazon API Gateway
        ↓
AWS Lambda Functions
        ↓
Amazon DynamoDB Table
        ↓
Unused IAM Roles / Policies
```

After completing the cleanup process, review the AWS Management Console to ensure that resources created specifically for CloudMenu are no longer active or unnecessarily maintained.

---

### Result

The cleanup procedure identifies and removes AWS resources that are no longer required after the deployment and evaluation of CloudMenu have been completed.

Cleaning up resources helps to:

- Prevent unexpected AWS costs.
- Avoid maintaining unnecessary testing resources.
- Keep the AWS environment organized and easier to manage.
- Remove IAM Roles and Policies that are no longer required.
- Reduce unnecessary resources in the AWS account.

In the current Workshop environment, CloudMenu resources are still maintained for testing, demonstration, and evaluation purposes. Resource cleanup will be performed after the evaluation process has been completed.