---
title: "Week 4 Worklog"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives

- Learn about AWS Lambda and backend processing using Serverless Architecture.
- Understand the structure of Lambda Functions, Events, Handlers, and Responses.
- Practice creating and testing Lambda Functions on AWS.
- Learn how AWS IAM Roles grant Lambda permission to access Amazon DynamoDB.
- Practice building basic backend functions for order processing in the CloudMenu system.

**Duration:** 13/07/2026 - 17/07/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Learn about **AWS Lambda** <br> + Understand the concept of Function as a Service (FaaS) <br> + Learn how Lambda works in Serverless Architecture <br> + Explore Runtime, Function, Event, and Handler | 13/07/2026 | 13/07/2026 | [https://aws.amazon.com/lambda/](https://aws.amazon.com/lambda/) |
| 2 | - Practice using **AWS Lambda** <br> + Create a Lambda Function <br> + Use Python as the Runtime <br> + Learn the structure of `lambda_handler` <br> + Create Test Events and verify Function execution results | 14/07/2026 | 14/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) |
| 3 | - Learn how Lambda accesses **Amazon DynamoDB** <br> + Use the AWS SDK for Python (**Boto3**) <br> + Perform read and write operations on DynamoDB from Lambda <br> + Verify stored data after Lambda execution | 15/07/2026 | 15/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html) |
| 4 | - Learn about **AWS IAM Roles** for Lambda <br> + Understand Execution Roles and IAM Policies <br> + Grant the required permissions for Lambda to access DynamoDB <br> + Verify access between Lambda and the `CloudMenuOrders` table | 16/07/2026 | 16/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) |
| 5 | - Build Lambda Functions for **CloudMenu** <br> + Build `createOrder` to create orders <br> + Build `getOrders` to retrieve order data <br> + Build `updateOrderStatus` to update order status <br> + Test read and write operations with the `CloudMenuOrders` table | 17/07/2026 | 17/07/2026 | [https://docs.aws.amazon.com/lambda/latest/dg/](https://docs.aws.amazon.com/lambda/latest/dg/) |

---

### Week 4 Achievements

- Understood the role of AWS Lambda in building a backend using Serverless Architecture.
- Learned the fundamental components of a Lambda Function, including Runtime, Event, Handler, and Response.
- Practiced creating, configuring, and testing Lambda Functions on AWS.
- Understood how Boto3 can be used by Lambda to read and write data in Amazon DynamoDB.
- Understood the role of IAM Execution Roles and Policies in granting Lambda access to DynamoDB.
- Built the `createOrder`, `getOrders`, and `updateOrderStatus` Lambda Functions for CloudMenu order processing.
- Prepared the backend for integration with Amazon API Gateway in the following weeks.

---