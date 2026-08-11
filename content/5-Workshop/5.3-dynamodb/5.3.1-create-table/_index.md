---
title: "Create the CloudMenuOrders Table"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.3.1. </b>"
---

## 5.3.1. Create the CloudMenuOrders Table

In this step, we will create an Amazon DynamoDB table to store order data for the CloudMenu system.

### Step 1: Open Amazon DynamoDB

Sign in to the **AWS Management Console**.

In the AWS Management Console search bar, enter:

`DynamoDB`

Select **DynamoDB** to open the Amazon DynamoDB console.

Make sure the selected Region is:

**Asia Pacific (Singapore) – ap-southeast-1**

### Step 2: Create the Table

In the Amazon DynamoDB console:

1. Select **Tables** from the left navigation menu.
2. Select **Create table**.
3. Enter the table configuration.

Configure the table as follows:

| Property | Value |
| :--- | :--- |
| **Table name** | `CloudMenuOrders` |
| **Partition key** | `orderId` |
| **Data type** | String |
| **Sort key** | Not used |
| **Capacity mode** | On-demand |

The `orderId` attribute is used as the Partition Key to uniquely identify each order in CloudMenu.

CloudMenu does not currently use a Sort Key for the `CloudMenuOrders` table.

With **On-demand capacity mode**, DynamoDB automatically manages read and write capacity according to application traffic without requiring predefined Read Capacity Units or Write Capacity Units. This configuration is suitable for the Workshop environment where traffic may vary and does not need to be predicted in advance.

After completing the configuration, select **Create table**.

### Step 3: Verify the Table

After DynamoDB finishes creating the table, navigate to:

**Tables → CloudMenuOrders**

Review the **General information** section.

![CloudMenuOrders table information](/images/5-Workshop/5.3/dynamodb-table-details.png)

The table is configured with:

- Partition key: `orderId (String)`
- Sort key: Not used
- Capacity mode: `On-demand`
- Table status: `Active`

When the table status becomes **Active**, the `CloudMenuOrders` table is ready to be used by the CloudMenu backend.

In the next step, we will review the structure of the order data stored in this table.