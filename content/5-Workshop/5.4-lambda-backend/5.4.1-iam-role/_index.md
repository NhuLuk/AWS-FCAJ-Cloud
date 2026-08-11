---
title: "IAM Execution Role"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---

## 5.4.1. IAM Execution Role

Each AWS Lambda function uses an IAM Execution Role to define which actions the function is allowed to perform on other AWS services.

In CloudMenu, Lambda requires permissions to:

- Write execution logs to Amazon CloudWatch Logs.
- Access the `CloudMenuOrders` table in Amazon DynamoDB.
- Be invoked by Amazon API Gateway through the `lambda:InvokeFunction` permission.

For the `createOrder` function, AWS Lambda uses the following Execution Role:

`createOrder-role-fmflntg9`

To review the Execution Role:

1. Open AWS Lambda.
2. Select the `createOrder` function.
3. Open the **Configuration** tab.
4. Select **Permissions**.
5. Review the **Execution role** section.

![Lambda Execution Role](/images/5-Workshop/5.4/lambda-execution-role.png)

The Execution Role should provide only the permissions required by the function.

The **Least Privilege** principle should be applied to limit each Lambda function to the AWS resources it needs to access.

After the Execution Role is configured, Lambda can write logs and interact with DynamoDB according to the granted permissions.