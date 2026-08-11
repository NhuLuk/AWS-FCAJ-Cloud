---
title: "Amazon S3"
date: 2026-06-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

## 5.6. Deploy the Frontend with Amazon S3

After completing the backend with Amazon API Gateway, AWS Lambda, and Amazon DynamoDB, the CloudMenu frontend is deployed to Amazon S3.

Amazon S3 is used to store static web assets such as HTML, CSS, and JavaScript files.

In the CloudMenu architecture, the S3 bucket is not directly exposed to the public. Instead, Amazon CloudFront is used to securely deliver the frontend content to users.

The frontend access flow is:

```text
User
  ↓
Amazon CloudFront
  ↓
Amazon S3
```

### Create the S3 Bucket

Open **AWS Management Console → Amazon S3** and create an S3 bucket for storing the CloudMenu frontend.

The bucket used in this workshop is:

```text
ozmr-s3-demo-bucket
```

The bucket is deployed in:

```text
Asia Pacific (Singapore) - ap-southeast-1
```

After creating the bucket, open it to upload the frontend files.

### Upload the Frontend to S3

Open the **Objects** tab, select **Upload**, and upload the CloudMenu frontend files.

The main frontend files include:

```text
app.js
index.html
order.html
style.css
frontend/
```

These files have the following purposes:

- `index.html`: provides the main CloudMenu interface.
- `order.html`: provides the order-related interface.
- `app.js`: handles frontend logic and communication with the backend API.
- `style.css`: defines the application's visual styles.
- `frontend/`: contains additional frontend resources.

After the upload is completed, the frontend files are stored as S3 objects.

![CloudMenu frontend files in S3](images/s3-objects.png)

### Verify the S3 Bucket Configuration

Open the **Properties** tab to verify the bucket configuration.

The `ozmr-s3-demo-bucket` bucket is deployed in:

```text
ap-southeast-1
```

Bucket Versioning is currently:

```text
Disabled
```

Versioning is not required for the scope of this workshop because the bucket is primarily used to store static frontend files.

![S3 bucket properties](images/s3-properties.png)

### Configure Bucket Access

The CloudMenu S3 bucket is not directly accessible to the public.

In the **Permissions** tab, **Block all public access** is enabled:

```text
Block all public access: On
```

This prevents users from directly accessing the frontend objects through public S3 access.

Instead, the bucket policy allows Amazon CloudFront to access the required objects in the bucket.

The policy uses the CloudFront service principal:

```text
cloudfront.amazonaws.com
```

With this configuration, Amazon S3 acts as a private origin while Amazon CloudFront is responsible for delivering the frontend content to users.

![S3 bucket permissions](images/s3-permissions.png)

### Result

The CloudMenu frontend is now successfully stored in Amazon S3.

The S3 bucket remains private and does not provide direct public access. The frontend content will instead be delivered through Amazon CloudFront.

The frontend access architecture is:

```text
User
  ↓
Amazon CloudFront
  ↓
Amazon S3
(Private Bucket)
```

In the next section, Amazon CloudFront will be configured to use the S3 bucket as its origin and distribute the CloudMenu frontend to users.