---
title: "Week 3 Worklog"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives

- Learn about the NoSQL database model and Amazon DynamoDB on AWS.
- Understand the main DynamoDB components, including Table, Item, Attribute, and Partition Key.
- Practice creating tables and adding, reading, and updating data in Amazon DynamoDB.
- Learn how to design an appropriate data structure for storing order information in the CloudMenu system.

**Duration:** 06/07/2026 - 10/07/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Learn about the **NoSQL** database model <br> + Understand the basic differences between relational and NoSQL databases <br> + Explore the characteristics and common use cases of NoSQL databases <br> + Learn the fundamentals of **Amazon DynamoDB** | 06/07/2026 | 06/07/2026 | [https://aws.amazon.com/dynamodb/](https://aws.amazon.com/dynamodb/) |
| 2 | - Learn the main components of **Amazon DynamoDB** <br> + Table <br> + Item <br> + Attribute <br> + Partition Key <br> + Understand how DynamoDB organizes and retrieves data | 07/07/2026 | 07/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 3 | - Practice using **Amazon DynamoDB** <br> + Create a DynamoDB Table <br> + Configure a Partition Key <br> + Add sample data to the Table <br> + Read and update Items through the AWS Management Console | 08/07/2026 | 08/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) |
| 4 | - Learn how to design order data in **DynamoDB** <br> + Identify the information required for an order <br> + Design a data structure containing Order ID, Table Number, Items, Total Amount, and Status <br> + Learn how a list of ordered items can be stored within a DynamoDB Item | 09/07/2026 | 09/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) |
| 5 | - Design the data model for **CloudMenu** <br> + Create the `CloudMenuOrders` table <br> + Use `orderId` as the Partition Key <br> + Define the attributes `tableNumber`, `items`, `totalAmount`, `status`, `createdAt`, and `updatedAt` <br> + Test storing and retrieving order data | 10/07/2026 | 10/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |

---

### Week 3 Achievements

- Understood the NoSQL database model and the role of Amazon DynamoDB in Serverless Architecture.
- Learned the fundamental DynamoDB components, including Table, Item, Attribute, and Partition Key.
- Practiced creating tables and adding, reading, and updating data in Amazon DynamoDB.
- Designed an order data structure suitable for the CloudMenu system.
- Created the `CloudMenuOrders` table with `orderId` as the Partition Key and prepared the data layer for integration with AWS Lambda in the following weeks.