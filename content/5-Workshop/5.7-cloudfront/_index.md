---
title: "Deploying Amazon CloudFront"
date: 2026-06-22
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

## 5.7. Deploying Amazon CloudFront

After uploading the frontend source files to Amazon S3, the next step is to configure **Amazon CloudFront** to distribute the CloudMenu frontend to users.

In this architecture, Amazon S3 stores the static frontend files, while Amazon CloudFront acts as a Content Delivery Network (CDN) that receives user requests and distributes content from the S3 origin.

The frontend access flow is as follows:

```text
User
  |
  v
Amazon CloudFront
  |
  v
Amazon S3
  |
  v
/frontend
  |
  v
index.html
```

---

### Checking the CloudFront Distribution

The CloudFront Distribution used for the application is named:

```text
CloudMenu
```

After the Distribution is created successfully, CloudFront provides the following Distribution Domain Name:

```text
d3be9t7i3323e7.cloudfront.net
```

This domain is used to access the CloudMenu frontend.

![CloudFront Distribution](/images/5-Workshop/5.7/cloudfront-distribution.png)

*Figure 1. CloudFront Distribution of the CloudMenu system.*

The main Distribution configuration is summarized below:

| Configuration | Value |
|---|---|
| Distribution | CloudMenu |
| Distribution domain | `d3be9t7i3323e7.cloudfront.net` |
| Default root object | `index.html` |
| Supported HTTP versions | HTTP/2, HTTP/1.1, HTTP/1.0 |
| Standard logging | Off |
| Custom domain | Not configured |

---

### Configuring the Default Root Object

In **General → Settings**, configure:

```text
Default root object: index.html
```

![CloudFront General Settings](/images/5-Workshop/5.7/cloudfront-general-settings.png)

*Figure 2. CloudFront General Settings and Default Root Object configuration.*

The `index.html` file is configured as the Default Root Object.

When a user accesses the CloudFront domain without specifying a file name, CloudFront automatically requests `index.html` from the configured origin.

For example:

```text
https://d3be9t7i3323e7.cloudfront.net/
```

CloudFront distributes the content corresponding to:

```text
index.html
```

This configuration allows users to access the main CloudMenu frontend through the CloudFront domain without manually entering `/index.html`.

---

### Configuring Amazon S3 as the Origin

In the **Origins** tab, the Amazon S3 bucket is configured as the origin of the CloudFront Distribution.

The bucket used by CloudMenu is:

```text
ozmr-s3-demo-bucket
```

The Origin Path is configured as:

```text
/frontend
```

![CloudFront Origin](/images/5-Workshop/5.7/cloudfront-origin.png)

*Figure 3. Amazon S3 configured as the origin of the CloudFront Distribution.*

Configuring the Origin Path as `/frontend` allows CloudFront to retrieve frontend files from:

```text
s3://ozmr-s3-demo-bucket/frontend/
```

instead of retrieving objects from the root of the bucket.

Therefore, when CloudFront requests:

```text
index.html
```

the actual object requested from the S3 origin is:

```text
/frontend/index.html
```

---

### Protecting the S3 Origin

The S3 bucket has **Block Public Access** enabled. Therefore, users cannot directly access the frontend objects through public S3 access.

CloudFront is configured with the required permissions to retrieve objects from the S3 origin.

The access model is:

```text
User
   |
   v
CloudFront
   |
   v
Private S3 Bucket
```

Instead of:

```text
User
   |
   X
Direct public access to S3
```

This approach keeps the S3 bucket private while allowing the CloudMenu frontend to be distributed through Amazon CloudFront.

---

### Configuring the Behavior

In the **Behaviors** tab, the Distribution uses the default behavior:

```text
Path pattern: Default (*)
```

The Viewer Protocol Policy is configured as:

```text
Redirect HTTP to HTTPS
```

The Cache Policy is:

```text
Managed-CachingOptimized
```

![CloudFront Behavior](/images/5-Workshop/5.7/cloudfront-behavior.png)

*Figure 4. Behavior configuration of the CloudFront Distribution.*

With **Redirect HTTP to HTTPS** enabled, requests using HTTP are redirected to HTTPS before the content is delivered.

For example:

```text
http://d3be9t7i3323e7.cloudfront.net
```

is redirected to an HTTPS connection.

The `Managed-CachingOptimized` cache policy allows CloudFront to cache appropriate content at edge locations, reducing the number of requests that need to be sent back to the S3 origin.

---

### Frontend Distribution Flow

After the configuration is completed, the frontend distribution process works as follows:

```text
Browser
   |
   | HTTPS Request
   v
Amazon CloudFront
   |
   | Check cached content
   |
   +------ Cache Hit ------> Return content
   |
   | Cache Miss
   v
Amazon S3
ozmr-s3-demo-bucket
   |
   v
/frontend
   |
   v
index.html / CSS / JavaScript
   |
   v
CloudFront
   |
   v
Browser
```

If the requested content is already available in the CloudFront cache, CloudFront can return it directly to the user.

If the content is not available in the cache, CloudFront retrieves the object from the S3 origin, delivers it to the client, and may cache the appropriate content at the edge location for subsequent requests.

---

### Result

After completing this step, the frontend distribution layer of CloudMenu is configured as follows:

```text
User
   |
   v
Amazon CloudFront
   |
   v
Amazon S3
   |
   v
CloudMenu Frontend
```

Amazon CloudFront provides an HTTPS endpoint for the frontend, while Amazon S3 continues to store the application's static assets.

Through CloudFront, users can access the CloudMenu frontend using the Distribution Domain Name without directly accessing the S3 bucket.

Testing access to the website through the CloudFront domain and verifying the connection between the frontend and backend API will be performed in **Section 5.8. System Testing**.