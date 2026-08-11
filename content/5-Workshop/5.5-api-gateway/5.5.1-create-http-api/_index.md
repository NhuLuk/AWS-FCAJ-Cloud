---
title: "Create the HTTP API"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.5.1. </b>"
---

## 5.5.1. Create the HTTP API

In this step, we will use Amazon API Gateway to create an HTTP API that serves as the entry point for the CloudMenu backend.

API Gateway receives HTTP requests from the frontend and forwards them to the appropriate AWS Lambda function for processing.

### Step 1: Open Amazon API Gateway

Sign in to the **AWS Management Console**.

In the search bar, enter:

`API Gateway`

Select **API Gateway** to open the API management console.

### Step 2: Create an HTTP API

In the Amazon API Gateway console:

1. Select **APIs**.
2. Select **Create API**.
3. Under **HTTP API**, select **Build**.
4. Enter the API name:

`CloudMenuAPI`

CloudMenu uses an HTTP API because the backend primarily provides HTTP endpoints for creating, retrieving, and updating orders.

After completing the configuration, create the API and open the `CloudMenuAPI` management page.

### Step 3: Verify the API

After the API is created, Amazon API Gateway provides an API endpoint that clients can use to send requests to the system.

In CloudMenu, the API acts as an intermediary in the following flow:

**Customer / Kitchen / Manager → API Gateway → Lambda → DynamoDB**

The application routes will be configured in the next step.

> **Note:** In the current CloudMenu environment, `CloudMenuAPI` is deployed in `us-east-1`, while the backend Lambda functions are deployed in `ap-southeast-1`. This Workshop preserves the configuration used by the current system.