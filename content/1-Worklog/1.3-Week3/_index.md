---
title: "Week 3 Worklog"
weight: 3
chapter: false
pre: "<b>1.3. </b>"
---

### Week 3 Objectives

- Learn about the NoSQL database model and Amazon DynamoDB on AWS.
- Understand the main components of DynamoDB, including Table, Item, Attribute, and Partition Key.
- Practice creating tables and adding, reading, and updating data in Amazon DynamoDB.
- Learn how to design an appropriate data structure for storing order information in the CloudMenu system.

**Duration:** 06/07/2026 - 10/07/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Learn about the **NoSQL** database model <br> + Understand the basic differences between relational databases and NoSQL databases <br> + Learn about the characteristics and common use cases of NoSQL <br> + Explore an overview of **Amazon DynamoDB** | 06/07/2026 | 06/07/2026 | [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) |
| 2 | - Learn about the main components of **Amazon DynamoDB** <br> + Table <br> + Item <br> + Attribute <br> + Partition Key <br> + Understand how DynamoDB organizes and retrieves data | 07/07/2026 | 07/07/2026 | [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) |
| 3 | - Practice using **Amazon DynamoDB** <br> + Create a DynamoDB Table <br> + Configure a Partition Key <br> + Add sample data to the Table <br> + Read and update Items using the AWS Management Console | 08/07/2026 | 08/07/2026 | [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) |
| 4 | - Learn how to design order data using **DynamoDB** <br> + Identify the information required for an order <br> + Design a data structure containing Order ID, Table Number, Items, Total Amount, and Status <br> + Learn how to store a list of dishes within a DynamoDB Item | 09/07/2026 | 09/07/2026 | [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) |
| 5 | - Design the data structure for the **CloudMenu** system <br> + Create the `CloudMenuOrders` table <br> + Use `orderId` as the Partition Key <br> + Define the attributes `tableNumber`, `items`, `totalAmount`, `status`, `createdAt`, `updatedAt`, and `completedAt` <br> + Verify the storage and retrieval of order data | 10/07/2026 | 10/07/2026 | [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) |

---

### Week 3 Achievements

- Understood the NoSQL database model and the role of Amazon DynamoDB in a Serverless architecture.
- Learned the fundamental components of DynamoDB, including Table, Item, Attribute, and Partition Key.
- Practiced creating tables and adding, reading, and updating data in Amazon DynamoDB.
- Understood how to select an appropriate Partition Key for order data.
- Designed an order data structure suitable for the business requirements of CloudMenu.
- Created the `CloudMenuOrders` table with `orderId` as the Partition Key.
- Defined the main order attributes, including `tableNumber`, `items`, `totalAmount`, `status`, `createdAt`, `updatedAt`, and `completedAt`.
- Prepared the data layer for integrating Amazon DynamoDB with AWS Lambda in the following implementation steps.