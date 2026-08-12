---
title : "VPC and Networking"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---

## 5.2.1 VPC and Networking

CloudMenu follows a serverless architecture in which most core components rely on AWS managed services. Therefore, the network design does not require every application resource to be deployed inside an Amazon VPC.

Instead of implementing a traditional three-tier architecture based on an Application Load Balancer, Amazon ECS, and Amazon RDS inside dedicated subnets, CloudMenu uses Amazon S3 and Amazon CloudFront for frontend delivery and Amazon API Gateway, AWS Lambda, and Amazon DynamoDB for backend processing.

AWS manages most of the underlying network infrastructure for these serverless services. As a result, services such as Amazon S3, CloudFront, API Gateway, and DynamoDB do not need to be deployed directly into the public or private subnets of the CloudMenu VPC.

The following networking components may become relevant as CloudMenu expands:

| Component | Role |
| :--- | :--- |
| **Amazon VPC** | Provides an isolated virtual network for AWS resources that require network-level isolation. In the current architecture, a VPC becomes necessary mainly when Lambda must access private resources or when additional VPC-based backend components are introduced. |
| **Internet Gateway** | Provides Internet connectivity for appropriate resources in public subnets. It is not used as a direct access path to Lambda or DynamoDB. |
| **NAT Gateway** | Allows resources located in private subnets to establish outbound Internet connections without assigning public IP addresses directly to those resources. |
| **VPC Endpoint** | Provides connectivity from resources inside a VPC to supported AWS services without requiring the traffic to depend on the public Internet path. |
| **Security Group** | Controls network traffic for resources with network interfaces inside a VPC, such as a VPC-connected Lambda function or private backend resources added in the future. |

### VPC and Subnet Design

Amazon VPC (Virtual Private Cloud) provides a logically isolated network environment in AWS. It allows the architecture to define IP address ranges, subnets, routing, gateways, and network security controls.

However, using AWS does not mean that every service must be placed inside a VPC.

In CloudMenu, frontend files are stored in Amazon S3 and distributed to users through Amazon CloudFront. These are AWS managed services and do not need to be placed inside CloudMenu subnets.

Similarly, Amazon API Gateway provides the API entry point for the frontend, while DynamoDB provides the managed NoSQL database layer. Neither service requires dedicated CloudMenu subnets for the current architecture.

AWS Lambda can also operate without being attached to a customer VPC. In the current implementation, Lambda can communicate with DynamoDB using AWS SDK operations and the required IAM permissions without requiring CloudMenu to deploy a NAT Gateway or a dedicated public/private subnet architecture.

This approach is suitable for the current development and testing environment because it reduces the number of networking components that need to be configured and maintained.

If Lambda needs to access private resources in the future, the architecture can be extended with a subnet design such as:

| Subnet Group | Number | Purpose |
| :--- | :---: | :--- |
| **Public Subnets** | 2 | One subnet can be deployed in each Availability Zone for components that require public routing or networking resources such as a NAT Gateway. |
| **Private Subnets** | 2 | One subnet can be deployed in each Availability Zone for Lambda or backend resources that require network isolation and should not receive direct inbound Internet traffic. |

This represents a possible future architecture and does not mean that the current CloudMenu implementation requires four subnets.

Public and private subnets should only be introduced when actual application resources need to run inside the VPC. This avoids unnecessary networking complexity while the current serverless architecture does not require subnet-level isolation.

### CloudMenu Network Flow

CloudMenu has two primary network flows: frontend content delivery and backend data processing.

For frontend content:

**Customer / Kitchen / Manager → Amazon CloudFront → Amazon S3**

Users access the application through CloudFront, which distributes HTML, CSS, JavaScript, images, and other static assets stored in Amazon S3.

For API and application data:

**Customer / Kitchen / Manager → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Within this flow:

- **Amazon CloudFront** distributes frontend content to users.
- **Amazon S3** stores frontend files and static assets.
- **Amazon API Gateway** receives HTTP requests from the frontend.
- **AWS Lambda** executes CloudMenu business logic.
- **Amazon DynamoDB** stores order information and is accessed through the backend rather than directly by the client.

This design separates frontend delivery from application data processing.

In particular, the Customer, Kitchen, and Manager browsers do not require direct permissions to the `CloudMenuOrders` table. The frontend sends requests to API Gateway, and Lambda performs the required DynamoDB operations using its assigned IAM permissions.

### Security Groups

A Security Group acts as a stateful virtual firewall for AWS resources that have network interfaces inside a VPC.

Inbound and outbound rules define the network traffic that is permitted for the associated resources.

In the current CloudMenu serverless architecture, Security Groups are not required for Amazon S3, Amazon CloudFront, Amazon API Gateway, or Amazon DynamoDB.

If Lambda remains outside the VPC, CloudMenu also does not need to create a Security Group solely for Lambda to communicate with DynamoDB.

Security Groups become relevant when the architecture is expanded and Lambda or other backend resources are deployed inside a VPC.

Potential Security Groups in a future architecture include:

| Security Group | Purpose |
| :--- | :--- |
| **Lambda Security Group** | Associated with Lambda when the function is connected to a VPC and used to control required network communication with private resources. |
| **Private Resource Security Group** | Can protect a database or another private service and restrict access to the Lambda Security Group or other approved sources. |
| **VPC Endpoint Security Group** | Can be associated with Interface VPC Endpoints to control HTTPS connectivity from resources inside the VPC. |

Security Groups should therefore be introduced according to actual network communication requirements rather than created before any resource requires them.

### VPC Endpoints

A VPC Endpoint provides connectivity between resources inside a VPC and supported AWS services without requiring the communication to depend entirely on the public Internet path.

VPC Endpoints are not mandatory in the current CloudMenu architecture because Lambda currently operates outside a VPC.

If Lambda is moved into private subnets in the future, VPC Endpoints can be considered to provide private connectivity to required AWS services and, in some scenarios, reduce dependency on a NAT Gateway.

Two important endpoint categories are:

- **Gateway Endpoint:** supported by selected AWS services such as Amazon S3 and associated with VPC route tables.
- **Interface Endpoint:** creates network interfaces inside the VPC and provides private connectivity to supported AWS services through AWS PrivateLink.

Possible endpoints for a future CloudMenu architecture include:

| Resource | Type | Operational Purpose |
| :--- | :--- | :--- |
| **S3 VPC Endpoint** | Gateway | Allows appropriate resources inside the VPC to access Amazon S3 through the endpoint instead of relying on an Internet path. |
| **CloudWatch Logs Endpoint** | Interface | Can provide private connectivity from VPC resources to CloudWatch Logs. |
| **ECR API Endpoint** | Interface | May be used if the backend is later deployed using container images and requires access to the Amazon ECR API. |
| **ECR DKR Endpoint** | Interface | Supports connectivity to the Amazon ECR Docker Registry for container-based workloads. |

The ECR-related endpoints are not required by the current CloudMenu implementation. They represent possible future components only if the backend evolves toward container-based workloads.

### NAT Gateway and Internet Access

A NAT Gateway is not required by the current CloudMenu architecture.

When Lambda operates outside a VPC, a NAT Gateway does not need to be created simply to allow the function to communicate with DynamoDB.

In a future architecture, if Lambda is deployed inside private subnets and also requires outbound Internet connectivity, a NAT Gateway may be deployed in a public subnet to provide the required outbound path.

However, introducing a NAT Gateway adds another infrastructure component to manage and may generate additional costs even for relatively low-traffic environments.

For this reason, CloudMenu should only introduce a NAT Gateway when a specific connectivity requirement exists rather than deploying one by default in the development environment.

### Future Networking Direction

During the current development stage, CloudMenu prioritizes a simple serverless architecture. Keeping Lambda outside a VPC when there are no private resources reduces the number of subnets, route tables, gateways, and Security Groups that need to be managed.

The current architecture can therefore remain based on:

**CloudFront → S3**

and:

**API Gateway → Lambda → DynamoDB**

As CloudMenu grows, requirements such as a private database, internal services, or stronger network isolation may justify introducing a dedicated VPC architecture.

At that stage, Lambda can be connected to private subnets, Security Groups can restrict network traffic, and VPC Endpoints or a NAT Gateway can be selected according to the actual connectivity requirements of the backend.

This approach allows CloudMenu to avoid unnecessary infrastructure complexity during the development stage while preserving a clear path toward stronger network isolation as security requirements, traffic, and system scale increase.