---
title: "IAM Execution Role"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---

## 5.4.1. IAM Execution Role

Each AWS Lambda Function uses an IAM Execution Role to define the permissions that the Function is allowed to use when accessing other AWS services.

In CloudMenu, Lambda requires the necessary permissions to:

- Write execution logs to Amazon CloudWatch Logs.
- Access the `CloudMenuOrders` table in Amazon DynamoDB according to the operations required by the system.

For the `createOrder` Function, AWS Lambda uses the following Execution Role:

`createOrder-role-fmflntg9`

The Execution Role can be checked by following these steps:

1. Open AWS Lambda.
2. Select the `createOrder` Function.
3. Select the **Configuration** tab.
4. Select **Permissions**.
5. Check the **Execution role** section.

![Lambda Execution Role](/images/5-Workshop/5.4/lambda-execution-role.png)

The Execution Role of `createOrder` has policies attached to provide permissions for logging and DynamoDB access.

These policies include:

- `AWSLambdaBasicExecutionRole`, which provides the permissions required for Lambda to write logs to Amazon CloudWatch Logs.
- `CloudMenuDynamoPolicy`, a custom policy used to grant access to the `CloudMenuOrders` table.

![IAM Role Policies](/images/5-Workshop/5.4/iam-role-policies.png)

The custom `CloudMenuDynamoPolicy` limits DynamoDB access to the required operations:

```text
dynamodb:PutItem
dynamodb:GetItem
dynamodb:UpdateItem
dynamodb:Scan