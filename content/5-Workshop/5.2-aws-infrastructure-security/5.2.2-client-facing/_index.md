---

title : "Frontend Hosting and User Authentication"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
------------------------

## 5.2.2 Frontend Hosting and User Authentication

This section describes how CloudMenu uses **Amazon S3** and **Amazon CloudFront** for frontend hosting and content delivery while also presenting the future authentication and authorization approach for the Customer, Kitchen, and Manager interfaces.

In the current architecture, the frontend is implemented as a static web application and stored in Amazon S3. Amazon CloudFront is placed in front of S3 to distribute frontend content to users over the Internet.

For authentication and authorization, CloudMenu keeps the current development and testing architecture relatively simple. When centralized account management and stronger role-based access control become necessary, Amazon Cognito can be introduced as an additional component.

### Amazon S3

**Role:** Amazon S3 stores the CloudMenu frontend files and static assets.

The stored content may include:

* Customer Interface.
* Kitchen Interface.
* Manager Dashboard.
* HTML files.
* CSS files.
* JavaScript files.
* Images and other static assets.

In this architecture, Amazon S3 provides the frontend storage layer and acts as the origin for Amazon CloudFront.

Users do not need to access the S3 bucket directly. Instead, the frontend is delivered through CloudFront.

The basic access flow is:

**User → Amazon CloudFront → Amazon S3**

After the developer updates the frontend, the latest files are deployed to S3 according to the project deployment process. Within the current CloudMenu scope, frontend files may be uploaded manually before CloudFront distributes the updated version to users.

**Why Amazon S3 was selected:** S3 is suitable for CloudMenu because the frontend mainly consists of static files and does not require server-side processing.

Benefits include:

* No dedicated web server is required.
* Suitable for static application assets.
* Easy integration with Amazon CloudFront.
* Can scale as frontend storage requirements increase.
* Suitable for cost-conscious development and testing environments.

### Amazon CloudFront

**Role:** Amazon CloudFront provides the content delivery layer in front of Amazon S3.

When Customer, Kitchen, or Manager users access CloudMenu, frontend requests are sent to CloudFront rather than directly to S3.

CloudFront retrieves content from the S3 origin when necessary and distributes it to users through the AWS edge network.

The frontend delivery flow is:

**Customer / Kitchen / Manager → CloudFront → S3**

CloudFront separates the public delivery endpoint from the frontend storage layer.

In addition to content distribution, CloudFront supports HTTPS and caching, reducing repeated requests for the same static assets from the S3 origin.

If the architecture needs to restrict direct access to the bucket, **Origin Access Control (OAC)** can be used so that CloudFront is allowed to access the required S3 objects while the bucket itself remains private.

**Why Amazon CloudFront was selected:** CloudFront provides several advantages for CloudMenu:

* CDN-based frontend distribution.
* HTTPS support.
* Reduced latency for content delivery.
* Caching of static assets.
* Reduced need to expose the S3 bucket directly to the Internet.
* Compatibility with the overall serverless architecture.

The combination of Amazon S3 and CloudFront allows CloudMenu to host and distribute its frontend without maintaining a traditional web server or Amazon EC2 instance.

### Frontend Configuration

The CloudMenu frontend needs to know the backend API address in order to send requests related to application data and business operations.

The backend endpoint is provided by Amazon API Gateway.

The communication flow can be represented as:

**Frontend → API Gateway Endpoint → AWS Lambda → DynamoDB**

The frontend can use an API Base URL when building requests to the backend.

For example:

```text
API_BASE_URL = API Gateway endpoint
```

These API requests may support operations such as:

* Creating an order.
* Retrieving orders.
* Updating order status.
* Retrieving data required by Customer, Kitchen, or Manager interfaces.

Separating the API endpoint from the main frontend logic makes it easier to use different backend environments.

For example, separate endpoints may be configured for:

* Development.
* Testing.
* Production.

In a complete build pipeline, the API Base URL can be supplied through environment variables or build configuration.

If the current CloudMenu frontend does not use a build system with environment-variable support, the endpoint can instead be maintained in a dedicated configuration file rather than being hard-coded across multiple JavaScript files.

### Authentication and Authorization

The three CloudMenu interfaces have different access requirements.

The **Customer Interface** is primarily designed for customers who access the application using a table-specific QR Code.

The **Kitchen Interface** and **Manager Dashboard**, however, provide internal functions such as processing orders, changing order statuses, and viewing management information.

Therefore, in a production environment, internal interfaces should not rely only on hidden URLs or on users knowing the correct path.

Authentication determines who the user is, while authorization determines what that user is allowed to do.

For example:

* Customer users may create orders and view the status associated with their table.
* Kitchen users may view incoming orders and update preparation status.
* Manager users may access dashboards and operational statistics.

In the current architecture, CloudMenu does not require a complex account-management system. This keeps the Workshop implementation simple.

As security requirements grow, Amazon Cognito can be integrated.

### Future Amazon Cognito Integration

Amazon Cognito is an AWS managed authentication service that can be used to provide centralized user management for CloudMenu.

A future implementation may include:

* **User Pool:** manages user registration, authentication, and accounts.
* **App Client:** allows the frontend to communicate with the User Pool.
* **JWT Tokens:** issued after successful authentication.
* **Groups or roles:** differentiate Kitchen, Manager, or other user categories.
* **API authorization:** validate authenticated requests before protected APIs are accessed.

A possible authentication flow is:

**Kitchen/Manager → Cognito → JWT Token → API Gateway → Lambda**

After a successful sign-in, the frontend receives a token from Cognito.

The token can be attached to API requests, and API Gateway or backend logic can then use the authentication information to determine whether the request should be allowed.

### Why Amazon Cognito Can Be Used

Amazon Cognito is suitable for future CloudMenu authentication requirements because:

* It is an AWS managed authentication service.
* It reduces the need to build and maintain a complete login system manually.
* It supports token-based authentication.
* It can integrate with frontend applications and API Gateway.
* It supports centralized user management.
* It can support groups or role-based access patterns.

However, Cognito should not be introduced simply to increase the number of AWS services used by the project.

If CloudMenu does not currently require centralized account management, Cognito can remain a future enhancement rather than a mandatory dependency of the current application.

### Current Security Scope

Even without Cognito, CloudMenu should maintain several basic security practices:

* The frontend must not store AWS Access Keys or Secret Access Keys.
* The frontend must not access DynamoDB directly.
* Database operations should pass through API Gateway and Lambda.
* Lambda should access DynamoDB using an IAM Role.
* S3 permissions should be appropriately restricted.
* If CloudFront is used with a private S3 origin, OAC can be configured.
* Internal APIs should be considered for authentication before production deployment.

It is important to distinguish two different security layers:

**IAM** controls permissions between AWS services.

**Authentication and Authorization** control what application users are allowed to access.

These mechanisms solve different problems and should not be treated as interchangeable.

### Future Direction

In future development, CloudMenu can evolve toward the following frontend architecture:

**User → CloudFront → S3**

for frontend content delivery, and:

**Authenticated User → Cognito → API Gateway → Lambda → DynamoDB**

for protected application functions.

Customer users may continue to use a simple QR-based access experience, while Kitchen and Manager users can be required to authenticate before accessing internal functions.

This approach allows CloudMenu to preserve a simple customer experience while providing a clear path toward stronger access control as the system grows.
