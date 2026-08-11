---
title: "Deploy Amazon CloudFront"
date: 2026-06-22
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

## 5.7. Deploy Amazon CloudFront

After uploading the frontend source files to Amazon S3, the next step is to configure **Amazon CloudFront** to distribute the CloudMenu frontend to users.

In this architecture, Amazon S3 stores the static frontend files, while Amazon CloudFront acts as the Content Delivery Network (CDN), receiving user requests and delivering content from the S3 origin.

The frontend delivery flow is implemented as follows:

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

### Verify the CloudFront Distribution

The CloudFront Distribution used by the application is named:

```text
CloudMenu
```

After the Distribution was created, CloudFront provided the following Distribution Domain Name:

```text
d3be9t7i3323e7.cloudfront.net
```

This domain is used as the frontend access endpoint for the CloudMenu application.

![CloudFront Distribution](images/cloudfront-distribution.png)

The main Distribution settings are:

| Configuration | Value |
|---|---|
| Distribution | CloudMenu |
| Distribution domain | `d3be9t7i3323e7.cloudfront.net` |
| Default root object | `index.html` |
| Supported HTTP versions | HTTP/2, HTTP/1.1, HTTP/1.0 |
| Standard logging | Off |
| Custom domain | Not configured |

### Configure the Default Root Object

Under **General → Settings**, the following value is configured:

```text
Default root object: index.html
```

![CloudFront General Settings](images/cloudfront-general-settings.png)

The `index.html` file is configured as the Default Root Object so that when users access the CloudFront domain without specifying a file name, CloudFront automatically requests `index.html` from the origin.

For example:

```text
https://d3be9t7i3323e7.cloudfront.net/
```

CloudFront delivers the content corresponding to:

```text
index.html
```

This allows users to access the CloudMenu frontend through the main CloudFront domain without manually specifying `/index.html`.

### Configure Amazon S3 as the Origin

In the **Origins** tab, the Amazon S3 bucket is configured as the origin of the CloudFront Distribution.

The configured bucket is:

```text
ozmr-s3-demo-bucket
```

The Origin Path is:

```text
/frontend
```

![CloudFront Origin](images/cloudfront-origin.png)

By setting the Origin Path to `/frontend`, CloudFront retrieves frontend files from:

```text
s3://ozmr-s3-demo-bucket/frontend/
```

instead of retrieving objects from the root of the bucket.

Therefore, when CloudFront requests:

```text
index.html
```

the actual object is retrieved from:

```text
/frontend/index.html
```

inside the S3 bucket.

### Protect the S3 Origin

The S3 bucket is configured with **Block Public Access**, preventing users from directly accessing frontend objects through public S3 access.

CloudFront is configured with origin access permissions so that it can retrieve the required objects from the private S3 bucket.

The access flow follows this model:

```text
User
   |
   v
CloudFront
   |
   v
Private S3 Bucket
```

instead of:

```text
User
   |
   X
Direct public access to S3
```

This approach keeps the S3 bucket private while allowing the frontend content to be delivered through CloudFront.

### Configure CloudFront Behavior

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

![CloudFront Behavior](images/cloudfront-behavior.png)

With **Redirect HTTP to HTTPS**, requests using HTTP are redirected to HTTPS before the content is delivered.

For example:

```text
http://d3be9t7i3323e7.cloudfront.net
```

is redirected to a secure HTTPS connection.

The `Managed-CachingOptimized` cache policy allows CloudFront to cache appropriate static content at edge locations, reducing the number of requests that need to reach the S3 origin.

### Frontend Delivery Flow

After the configuration is completed, frontend content is delivered through the following flow:

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

If the requested content is already available in the CloudFront cache, CloudFront can return it directly to the user. Otherwise, CloudFront retrieves the object from the S3 origin and delivers it to the client.

### Result

After this step, CloudMenu has completed the frontend distribution layer:

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

CloudFront provides an HTTPS endpoint for the frontend, while Amazon S3 continues to store the application's static assets.

Testing access through the CloudFront domain and verifying the connection between the frontend and backend API will be performed in **Section 5.8. Testing**.