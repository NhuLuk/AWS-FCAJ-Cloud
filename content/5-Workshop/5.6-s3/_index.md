---
title: "Amazon S3"
date: 2026-06-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

## 5.6. Deploying the Frontend with Amazon S3

After completing the backend using Amazon API Gateway, AWS Lambda, and Amazon DynamoDB, the CloudMenu frontend is deployed to Amazon S3.

Amazon S3 is used to store static frontend files such as HTML, CSS, and JavaScript.

In the CloudMenu architecture, the S3 bucket is not directly exposed to the public. Instead, Amazon CloudFront is used to distribute the frontend content to users.

The frontend access flow is as follows:

```text
User
  ↓
Amazon CloudFront
  ↓
Amazon S3
```

---

### Creating the S3 Bucket

Open **AWS Management Console → Amazon S3** and create an S3 bucket to store the CloudMenu frontend.

The bucket used in this workshop is:

```text
ozmr-s3-demo-bucket
```

The bucket is created in the following AWS Region:

```text
Asia Pacific (Singapore) - ap-southeast-1
```

After creating the bucket, open it to upload the CloudMenu frontend files.

---

### Uploading the Frontend to S3

In the **Objects** tab, select **Upload** and upload the CloudMenu frontend files to the bucket.

The main files include:

```text
app.js
index.html
order.html
style.css
frontend/
```

The purpose of each file is as follows:

- `index.html`: the main CloudMenu user interface.
- `order.html`: the interface related to order information.
- `app.js`: handles frontend logic and communication with the backend API.
- `style.css`: defines the styles of the application.
- `frontend/`: contains additional frontend resources.

After the upload process is completed, the files are stored as S3 Objects in the bucket.

![CloudMenu frontend files in S3](/images/5-Workshop/5.6/s3-objects.png)

*Figure 1. CloudMenu frontend files stored in Amazon S3.*

---

### Checking the S3 Bucket Configuration

In the **Properties** tab, review the configuration information of the bucket.

The `ozmr-s3-demo-bucket` bucket is deployed in:

```text
ap-southeast-1
```

Bucket Versioning is currently configured as:

```text
Disabled
```

For the scope of this workshop, Versioning is not required because the bucket is mainly used to store the static frontend files of CloudMenu.

![S3 bucket properties](/images/5-Workshop/5.6/s3-properties.png)

*Figure 2. Configuration information of the CloudMenu S3 bucket.*

---

### Configuring Access Permissions

CloudMenu does not allow users to access objects in the S3 bucket directly.

In the **Permissions** tab, **Block all public access** is enabled:

```text
Block all public access: On
```

This configuration prevents the frontend files stored in the bucket from being publicly accessed directly through Amazon S3.

Instead, Amazon CloudFront is used as the content distribution layer. The S3 bucket acts as a private origin for the CloudFront distribution.

The bucket policy allows Amazon CloudFront to access the required objects stored in the bucket.

The CloudFront service principal is:

```text
cloudfront.amazonaws.com
```

With this configuration, users access the CloudMenu frontend through Amazon CloudFront rather than directly accessing the S3 bucket.

The access flow can be represented as follows:

```text
User
  ↓
Amazon CloudFront
  ↓
Private S3 Bucket
  ↓
Frontend Files
```

![S3 bucket permissions](/images/5-Workshop/5.6/s3-permissions.png)

*Figure 3. Access permissions of the CloudMenu S3 bucket.*

---

### Result

After completing this step, the CloudMenu frontend is successfully stored in Amazon S3.

The S3 bucket remains private and does not allow direct public access. The frontend content will be distributed through Amazon CloudFront in the next step.

The frontend architecture after completing the S3 deployment is:

```text
CloudMenu Frontend
        ↓
Amazon S3
  (Private Bucket)
        ↓
Amazon CloudFront
        ↓
       User
```

Amazon S3 provides a simple storage solution for the static assets of the CloudMenu frontend, while keeping the bucket private helps prevent direct public access to the stored objects.

In the next section, Amazon CloudFront will be configured to use the S3 bucket as its origin and distribute the CloudMenu frontend to users.