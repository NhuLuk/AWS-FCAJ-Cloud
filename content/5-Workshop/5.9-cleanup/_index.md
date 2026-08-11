---
title: "Resource Cleanup"
date: 2026-06-22
weight: 9
chapter: false
pre: "<b>5.9. </b>"
---

## 5.9. Resource Cleanup

After completing the deployment and testing of CloudMenu, AWS resources that are no longer required should be cleaned up to prevent unexpected charges.

The main AWS services used in this workshop include:

- Amazon CloudFront
- Amazon S3
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB

> **Note:** Carefully verify your resources and data before deleting them because some deletion operations cannot be undone.

### 1. Delete the CloudFront Distribution

The CloudFront Distribution is used to deliver the CloudMenu frontend from Amazon S3.

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

Before deleting the Distribution, disable it first.

Select:

```text
CloudMenu
→ Disable
```

Wait until the Distribution update is completed, then select:

```text
Delete
```

Deleting the CloudFront Distribution stops the delivery of the CloudMenu frontend through CloudFront.

![Delete CloudFront Distribution](images/cleanup-cloudfront.png)

---

### 2. Delete Objects from the S3 Bucket

Amazon S3 is used to store the CloudMenu frontend files.

The bucket used in this workshop is:

```text
ozmr-s3-demo-bucket
```

Navigate to:

```text
Amazon S3
→ Buckets
→ ozmr-s3-demo-bucket
```

In the **Objects** tab, select the frontend objects and folders and choose:

```text
Delete
```

These files may include:

```text
frontend/
index.html
order.html
app.js
style.css
```

Confirm the deletion of the selected objects.

![Delete S3 Objects](images/cleanup-s3-objects.png)

---

### 3. Delete the S3 Bucket

After deleting the objects, return to the S3 bucket list.

Select:

```text
ozmr-s3-demo-bucket
```

Then choose:

```text
Delete
```

Enter the bucket name for confirmation if required:

```text
ozmr-s3-demo-bucket
```

After the operation is completed, the bucket used to store the CloudMenu frontend will be removed.

![Delete S3 Bucket](images/cleanup-s3-bucket.png)

---

### 4. Delete the API Gateway API

CloudMenu uses an HTTP API to connect the frontend to the Lambda Functions.

Navigate to:

```text
AWS Management Console
→ API Gateway
→ APIs
```

Select the CloudMenu API.

The API contains the following routes:

```text
POST /order
GET /orders
PUT /orders/{orderId}
```

Choose:

```text
Delete
```

and confirm the deletion.

![Delete API Gateway](images/cleanup-api-gateway.png)

---

### 5. Delete Lambda Functions

AWS Lambda is used to process the backend logic of CloudMenu.

Navigate to:

```text
AWS Management Console
→ Lambda
→ Functions
```

The functions used in CloudMenu include:

```text
createOrder
getOrders
updateOrderStatus
```

Select each Lambda Function that is no longer required and choose:

```text
Actions
→ Delete
```

Confirm the deletion.

![Delete Lambda Functions](images/cleanup-lambda.png)

---

### 6. Delete the DynamoDB Table

Amazon DynamoDB is used to store CloudMenu order data.

Navigate to:

```text
AWS Management Console
→ DynamoDB
→ Tables
```

Select:

```text
CloudMenuOrders
```

Then choose:

```text
Delete
```

Confirm the operation to delete the table.

> Deleting the DynamoDB table also removes the order data stored in the table. Only perform this step when the data is no longer required.

![Delete DynamoDB Table](images/cleanup-dynamodb.png)

---

### Recommended Cleanup Order

The resources can be cleaned up in the following order:

```text
CloudFront Distribution
        ↓
S3 Objects
        ↓
S3 Bucket
        ↓
API Gateway
        ↓
Lambda Functions
        ↓
DynamoDB Table
```

After completing the cleanup process, check the AWS Management Console again to ensure that the resources created specifically for the CloudMenu workshop have been removed or are no longer in use.

### Result

The cleanup process removes AWS resources that are no longer required after completing the workshop.

Cleaning up AWS resources helps to:

- Prevent unexpected AWS charges.
- Avoid keeping unused experimental resources.
- Maintain a clean and manageable AWS environment.

At this point, the deployment, testing, and cleanup process for **CloudMenu** is complete.