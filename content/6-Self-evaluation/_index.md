---
title: "Self-evaluation"
date: 2026-06-22
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## 6. Self-evaluation

During my internship at **AMAZON WEB SERVICES VIETNAM COMPANY LIMITED** from **22/06/2026 to 15/08/2026**, I had the opportunity to participate in the **First Cloud AI Journey (FCAJ)** program and work on **CloudMenu** – a table-side online ordering system built and deployed on Amazon Web Services (AWS).

CloudMenu allows customers to use their mobile devices to access the menu through a QR Code assigned to each table, select dishes, submit orders, and track order status. In addition to the customer interface, the system also provides a Kitchen Interface for kitchen staff to receive orders and update their status, as well as a Dashboard for monitoring system statistics.

During the internship, I learned and applied knowledge related to cloud computing, Serverless architecture, NoSQL databases, and web application development to gradually build CloudMenu.

The main AWS services used in the project include:

- **Amazon S3:** Stores the frontend files of CloudMenu.
- **Amazon CloudFront:** Distributes frontend content to users over HTTPS.
- **Amazon API Gateway:** Provides an HTTP API for communication between the frontend and backend.
- **AWS Lambda:** Handles business logic related to orders.
- **Amazon DynamoDB:** Stores the order data of the system.
- **AWS Identity and Access Management (IAM):** Manages access permissions between AWS services.

### Tasks Performed

During the CloudMenu project, I participated in the following tasks:

- Learned about AWS services and the Serverless architecture model.
- Analyzed requirements and identified the main features of the CloudMenu system.
- Learned about Amazon DynamoDB and designed the order data structure.
- Created the `CloudMenuOrders` table and used `orderId` as the Partition Key.
- Learned about AWS Lambda and developed Lambda Functions for order processing.
- Developed the `createOrder` Function to create new orders.
- Developed the `getOrders` Function to retrieve the list of orders.
- Developed the `updateOrderStatus` Function to update order status.
- Connected AWS Lambda to Amazon DynamoDB using the AWS SDK for Python (`boto3`).
- Configured IAM Execution Roles and the required permissions for Lambda to access DynamoDB and write logs.
- Built an HTTP API using Amazon API Gateway.
- Created the `POST /order`, `GET /orders`, and `PUT /orders/{orderId}` routes.
- Integrated API Gateway routes with the corresponding Lambda Functions.
- Tested the APIs and resolved issues that occurred during the integration process.
- Deployed the CloudMenu frontend to Amazon S3.
- Used Amazon CloudFront to distribute the frontend to users.
- Built and tested the Customer Interface for viewing the menu and placing orders.
- Built and tested the Kitchen Interface for monitoring and updating order status.
- Developed the order tracking feature for customers.
- Prepared the Worklog, Proposal, Workshop, architecture diagrams, processing flow diagrams, and other project-related documentation.

Through the CloudMenu project, I gained a better understanding of how AWS services can be integrated to build a complete Serverless application. I also gained more experience in deploying and integrating frontend, API, backend, and database components in an AWS environment.

In addition to technical knowledge, the internship also helped me improve my self-learning, documentation research, problem analysis, troubleshooting, time management, communication, and teamwork skills.

### Self-evaluation Table

| No. | Criteria | Description | Good | Fair | Average |
| --- | --- | --- | :---: | :---: | :---: |
| 1 | **Professional knowledge and technical skills** | Able to apply Amazon S3, CloudFront, API Gateway, Lambda, DynamoDB, and IAM to build and deploy CloudMenu. | ✅ | ☐ | ☐ |
| 2 | **Learning ability** | Proactively learned about AWS services, Serverless architecture, and new knowledge during the internship. | ☐ | ✅ | ☐ |
| 3 | **Initiative** | Proactively researched documentation, practiced, and looked for solutions when problems occurred during deployment. | ✅ | ☐ | ☐ |
| 4 | **Responsibility** | Took responsibility for assigned tasks and made an effort to complete work according to the program schedule. | ✅ | ☐ | ☐ |
| 5 | **Discipline** | Followed the schedule, program requirements, and internship report preparation process. | ☐ | ✅ | ☐ |
| 6 | **Willingness to improve** | Accepted feedback and proactively improved knowledge and project quality. | ✅ | ☐ | ☐ |
| 7 | **Communication** | Communicated with team members and mentors when difficulties occurred during the project. | ☐ | ✅ | ☐ |
| 8 | **Teamwork** | Collaborated with team members during the analysis, development, testing, and completion of CloudMenu. | ✅ | ☐ | ☐ |
| 9 | **Professional behavior** | Maintained a serious and respectful attitude and took responsibility for assigned work. | ✅ | ☐ | ☐ |
| 10 | **Problem-solving skills** | Able to identify causes and resolve issues related to AWS configuration and the integration of API Gateway, Lambda, and DynamoDB. | ☐ | ✅ | ☐ |
| 11 | **Project contribution** | Participated in developing CloudMenu, deploying AWS components, testing the system, and completing Workshop documentation. | ✅ | ☐ | ☐ |
| 12 | **Overall performance** | Successfully completed the internship program activities and gained practical knowledge and skills related to AWS Cloud. | ✅ | ☐ | ☐ |

### Knowledge and Skills Acquired

After the internship, I gained several important areas of knowledge and skills:

- Gained a better understanding of AWS Cloud fundamentals and Serverless architecture.
- Understood the roles of Amazon S3 and Amazon CloudFront in storing and distributing frontend content.
- Learned how to build a Serverless backend using Amazon API Gateway and AWS Lambda.
- Learned how to use Amazon DynamoDB to store and retrieve data.
- Understood how IAM Roles and Policies are used to manage access permissions between AWS services.
- Gained experience in API testing and troubleshooting issues during service integration.
- Understood the workflow of a web application from the frontend to the API, backend, and database.
- Gained more experience in preparing technical documentation and Workshop materials.
- Improved my ability to independently research and find solutions to technical problems.
- Improved my teamwork and task management skills.

### Areas for Improvement

In addition to the results achieved, I recognize that there are still several areas that I need to continue improving:

- Improve my knowledge of AWS architecture design for larger and more complex systems.
- Learn more about security, authentication, and authorization in Serverless applications.
- Improve my skills in using Amazon CloudWatch for system monitoring, log analysis, and troubleshooting.
- Learn more about performance and cost optimization methods for applications running on AWS.
- Learn more about Infrastructure as Code and CI/CD to automate the deployment process.
- Improve my testing skills and ability to handle different system failure scenarios.
- Improve my ability to present and explain technical solutions to team members and mentors.
- Continue improving my ability to read and understand technical documentation in English.

### Conclusion

The **First Cloud AI Journey** program gave me the opportunity to learn and gain hands-on experience with AWS services through a practical project. Through the development of CloudMenu, I not only gained a better understanding of Serverless architecture but also had the opportunity to practice the process of designing, deploying, integrating, and testing an application on a cloud platform.

The knowledge and experience gained during the internship provide an important foundation for me to continue developing my knowledge and skills in Cloud Computing, Software Development, and AWS technologies in the future.