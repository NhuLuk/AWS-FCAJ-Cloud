---

title: "Frontend Hosting"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
aliases:

- /5-workshop/5.3-deployment-operations-monitoring/5.3.2-frontend-hosting-auth/

---

## 5.3.2. Frontend Hosting

### Overview

CloudMenu uses **Amazon S3** to store frontend files and **Amazon CloudFront** to distribute application content to users over the Internet.

The frontend is implemented as a static web application consisting of HTML, CSS, JavaScript, images, and other static assets. Therefore, CloudMenu does not require a dedicated web server or an Amazon EC2 instance solely to host the user interface.

In the current architecture, Amazon S3 acts as the storage layer for frontend files, while Amazon CloudFront provides the content delivery layer in front of S3. Users access CloudMenu through the CloudFront domain instead of running the application directly from a development machine.

CloudMenu currently provides three primary frontend interfaces:

* **Customer Interface:** allows customers to access the table-specific menu through a QR Code, select items, manage the cart, submit orders, and track order status.
* **Kitchen Interface:** allows kitchen staff to monitor incoming orders and update their processing status.
* **Manager Dashboard:** provides managers with an overview of order data, processing statuses, revenue, and operational statistics.

Although these interfaces serve different user roles, they are deployed as static frontend files using the same S3 and CloudFront hosting architecture.

### Frontend Hosting Architecture

The CloudMenu frontend architecture is illustrated below:

![Frontend Architecture](/images/5-Workshop/AWS_CloudMenu_Frontend.png)

The basic access flow can be summarized as:

**Customer/Kitchen/Manager → Browser → Amazon CloudFront → Amazon S3**

When a user accesses the CloudFront domain, the browser sends an HTTPS request to Amazon CloudFront.

CloudFront serves the requested frontend content to the user. If the requested content is not currently available in the appropriate edge cache, CloudFront retrieves the file from the Amazon S3 origin and then distributes it to the browser.

Frontend files stored in S3 may include:

* HTML files.
* CSS files.
* JavaScript files.
* Menu item images.
* Icons and other static assets.
* Customer, Kitchen, and Manager interface files.

With this architecture, the CloudMenu frontend no longer depends on the development workstation. The developer does not need to keep Visual Studio Code or a local development server running for users to access the application.

### Amazon S3 Role

Amazon S3 stores the static frontend assets used by CloudMenu.

After the frontend is completed or updated, the required files are uploaded to the corresponding S3 bucket.

The bucket may contain files such as:

```text
index.html
kitchen.html
manager.html
css/
js/
images/
```

The actual file names and directory structure depend on the CloudMenu frontend implementation.

Amazon S3 is suitable for this use case because the frontend consists primarily of static assets and does not require server-side processing to deliver HTML, CSS, or JavaScript files.

### Amazon CloudFront Role

Amazon CloudFront is placed in front of Amazon S3 and distributes frontend content to users.

Instead of requiring users to access the S3 bucket directly, CloudMenu uses the CloudFront domain as the public frontend address.

Some benefits of using CloudFront include:

* Content delivery through AWS edge locations.
* HTTPS access for frontend users.
* Reduced repeated requests to S3 through caching.
* Separation between the frontend storage layer and the public access endpoint.
* Future support for custom domains and additional security configurations.

In CloudMenu, CloudFront handles frontend content delivery, while application data requests are sent separately from browser-side JavaScript to Amazon API Gateway.

Therefore, the following two flows should be distinguished:

**Frontend content:**

**Browser → CloudFront → S3**

**Application data:**

**Browser → API Gateway → Lambda → DynamoDB**

Amazon S3 and CloudFront do not directly process order creation or order-status update logic.

### Frontend Deployment Flow

CloudMenu currently **does not use an automated CI/CD pipeline for frontend deployment**.

The project source code is managed in a source repository, but deployment of updated frontend files to Amazon S3 is performed manually within the current Workshop scope.

The current deployment flow is illustrated below:

![Frontend Deployment Flow](/images/5-Workshop/AWS_CloudMenu_Frontend2.png)

The process can be summarized as:

**Developer → Source Code/GitHub → Local Development → Manual Upload → Amazon S3 → Amazon CloudFront → User**

After a frontend change is completed, the developer verifies the application in the local development environment before uploading the required files to S3.

CloudFront then continues to distribute the updated frontend through its public domain.

If previous versions of the frontend assets are already cached by CloudFront, it may be necessary to wait for the cache to expire or use an appropriate invalidation process to make the latest files available immediately.

### Why Manual Deployment Is Used in the Workshop

Manual frontend deployment is used in the current phase because the primary goal of CloudMenu is to understand and implement the complete application flow on AWS.

This approach provides several benefits in a development and Workshop environment:

* Simple deployment process.
* Each deployment step can be inspected manually.
* No additional CI/CD pipeline is required.
* Suitable when deployment frequency is relatively low.
* Allows the project to focus on the core AWS services.

However, manual deployment also introduces several limitations:

* Files must be uploaded again after each frontend update.
* Files may accidentally be omitted during deployment.
* An incorrect frontend version may be uploaded.
* There is no automated source-code validation before deployment.
* Rollback to a previous version is not automated.
* The process becomes harder to manage as deployment frequency increases.

In future development, CloudMenu can introduce CI/CD automation to build, validate, and synchronize frontend files from the source repository to Amazon S3.

### Verify the Frontend Deployment

After the frontend files have been uploaded to Amazon S3 and CloudFront is serving the latest content, the system can be tested through the CloudFront domain.

Example:

https://d3be9t7i3323e7.cloudfront.net/index.html?table=02

The query parameter:

```text
?table=02
```

is used to pass the table identifier to the Customer Interface when the application is opened through a corresponding QR Code or URL.

### Verify the Customer Interface

The Customer Interface should be tested using a URL that includes the table information.

![Customer](/images/5-Workshop/AWS_CloudMenu_Customer.png)

The following functions should be verified:

* The menu loads successfully through CloudFront.
* The correct table number is identified.
* Menu items are displayed correctly.
* Customers can add and remove items from the cart.
* The order total is calculated correctly.
* Orders can be submitted to the backend.
* Order status can be displayed after order creation.

Testing the Customer Interface confirms that the frontend is not only distributed successfully but is also able to communicate with the CloudMenu backend API.

### Verify the Kitchen Interface

The Kitchen Interface is used to verify that orders submitted by customers are processed by the backend and can be displayed to kitchen staff.

![Kitchen](/images/5-Workshop/AWS_CloudMenu_Kitchen.png)

The following items should be checked:

* The order list loads successfully.
* Table information is displayed correctly.
* Ordered items and quantities are available.
* The current order status is displayed.
* Kitchen staff can update order statuses.
* Updated data is reflected in the backend.

### Verify the Manager Dashboard

The Manager Dashboard is used to validate the management and reporting interface.

![Manager](/images/5-Workshop/AWS_CloudMenu_Manager.png)

The dashboard may include information such as:

* Total number of orders.
* Total revenue.
* Orders grouped by status.
* Table-related activity.
* Frequently ordered items.
* Other statistics derived from order data.

A correctly functioning Manager Dashboard confirms that the frontend can retrieve backend data and process it into the information required by restaurant management.

### Verify Frontend and Backend Integration

Frontend hosting should only be considered complete when both frontend distribution and backend communication work successfully.

The end-to-end flow can be represented as:

**CloudFront Frontend → API Gateway → Lambda → DynamoDB**

For example:

1. The Customer opens the menu through CloudFront.
2. The Customer creates a new order.
3. The frontend sends the request to API Gateway.
4. Lambda processes the request and stores the order in DynamoDB.
5. The Kitchen interface retrieves the order.
6. Kitchen staff update the order status.
7. The Customer or Manager interface receives the updated information.

If these steps work correctly, both frontend hosting and frontend-to-backend integration have been implemented successfully.

### Result

After completing this section, the Customer, Kitchen, and Manager interfaces can be accessed over the Internet using the CloudFront domain instead of depending on a local development server.

Amazon S3 provides frontend storage, while Amazon CloudFront provides the content delivery layer.

This design remains consistent with the overall serverless architecture of CloudMenu and provides the foundation for the end-to-end testing, monitoring, and optimization activities described in the following sections.
