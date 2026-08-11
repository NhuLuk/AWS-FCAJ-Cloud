---
title: "Week 5 Worklog"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives

- Learn about Amazon API Gateway and the role of APIs in Serverless Architecture.
- Understand basic HTTP methods and how to build REST APIs.
- Practice integrating Amazon API Gateway with AWS Lambda.
- Learn and configure CORS to enable communication between the CloudMenu frontend and backend.

**Duration:** 20/07/2026 - 24/07/2026

---

### Weekly Task Overview

| Day | Activities | Start Date | End Date | References |
| ---- | ---------- | ---------- | -------- | ---------- |
| 1 | - Learn about **Amazon API Gateway** <br> + Understand the role of API Gateway in Serverless Architecture <br> + Learn the concept of REST APIs <br> + Understand the communication flow between Client, API Gateway, and Backend | 20/07/2026 | 20/07/2026 | [https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/) |
| 2 | - Learn about **HTTP methods** <br> + Understand GET, POST, PUT, and OPTIONS <br> + Learn about Resources, Methods, Requests, and Responses <br> + Design the APIs required for order processing | 21/07/2026 | 21/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) |
| 3 | - Practice integrating **Amazon API Gateway with AWS Lambda** <br> + Create API Resources and Methods <br> + Connect APIs to Lambda Functions <br> + Test requests and responses between API Gateway and Lambda | 22/07/2026 | 22/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-rest-apis.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-rest-apis.html) |
| 4 | - Build order processing APIs for **CloudMenu** <br> + Configure `POST /order` to create orders <br> + Configure `GET /orders` to retrieve orders <br> + Configure `PUT /orders/{orderId}` to update order status <br> + Test the API Gateway → Lambda → DynamoDB flow | 23/07/2026 | 23/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/](https://docs.aws.amazon.com/apigateway/latest/developerguide/) |
| 5 | - Learn and configure **CORS** <br> + Learn about Same-Origin Policy and Cross-Origin Requests <br> + Configure the required HTTP Methods and Headers <br> + Connect the frontend to API Gateway <br> + Test the Frontend → API Gateway → Lambda → DynamoDB flow | 24/07/2026 | 24/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) |

---

### Week 5 Achievements

- Understood the role of Amazon API Gateway in Serverless Architecture.
- Learned the basic HTTP methods and the process of building REST APIs.
- Practiced integrating Amazon API Gateway with AWS Lambda.
- Built the `POST /order`, `GET /orders`, and `PUT /orders/{orderId}` APIs for CloudMenu order processing.
- Understood and configured CORS to allow the frontend to send requests to API Gateway.
- Successfully tested the data processing flow from API Gateway to Lambda and Amazon DynamoDB.
- Completed the backend API foundation required for integration with the CloudMenu interface in the following weeks.