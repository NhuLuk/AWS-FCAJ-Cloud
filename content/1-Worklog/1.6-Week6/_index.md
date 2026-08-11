---
title: "Week 6 Worklog"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives

- Analyze the requirements and main features of the CloudMenu system.
- Design the QR Code-based ordering process for restaurant tables.
- Build the customer interface for viewing the menu, selecting items, and submitting orders.
- Build the kitchen interface for monitoring and updating order status.

**Duration:** 27/07/2026 - 31/07/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Analyze the requirements of the **CloudMenu** system <br> + Identify the main users: Customer, Kitchen, and Manager/Admin <br> + Identify the required system features <br> + Analyze the ordering process from the customer to the kitchen | 27/07/2026 | 27/07/2026 | - |
| 2 | - Design the **QR Code** ordering feature <br> + Use a unique QR link for each table <br> + Use the `table` parameter in the URL to identify the table number <br> + Validate the table number before displaying the menu | 28/07/2026 | 28/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) |
| 3 | - Build the **CloudMenu customer interface** <br> + Display menu items <br> + Search and filter items by category <br> + Add items to the cart and change quantities <br> + Calculate the total order amount | 29/07/2026 | 29/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) |
| 4 | - Complete the order submission and tracking features <br> + Send order information from the frontend to the API <br> + Include the table number in each order <br> + Display order information and status to customers <br> + Handle the `PENDING`, `PREPARING`, and `COMPLETED` statuses | 30/07/2026 | 30/07/2026 | [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) |
| 5 | - Build the **Kitchen interface** <br> + Display the order list <br> + Display table number, ordered items, quantities, and total amount <br> + Update order status from `PENDING` to `PREPARING` and `COMPLETED` <br> + Test the Customer → Order Submission → Kitchen → Order Completion workflow | 31/07/2026 | 31/07/2026 | - |

---

### Week 6 Achievements

- Completed the requirement analysis and identified the main features of the CloudMenu system.
- Built the table identification mechanism using QR Codes and the `table` URL parameter.
- Built the customer interface for viewing the menu, searching, filtering items, and managing the shopping cart.
- Completed the order submission feature and implemented the `PENDING`, `PREPARING`, and `COMPLETED` order statuses.
- Built the Kitchen interface for viewing and updating order status.
- Completed the main workflow from customer ordering to kitchen processing and order completion.