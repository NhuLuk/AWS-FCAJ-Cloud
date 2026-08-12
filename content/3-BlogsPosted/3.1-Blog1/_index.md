---

title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
----------------------

# TURNING MILLIONS OF CONTRACTS INTO USEFUL DATA WITH DOCZY.AI AND GENERATIVE AI ON AWS

In many enterprises, especially in the healthcare and financial sectors, a large amount of important information is still stored in unstructured documents such as contracts, legal agreements, and invoices.

A single contract can contain dozens or even hundreds of pages with different structures, clauses, and terms. When the number of documents grows to thousands or millions, manually reading, extracting, and reviewing this information becomes time-consuming and difficult to scale.

While exploring real-world applications of Generative AI on AWS, I came across an interesting use case: **Doczy.ai by AArete**, a platform that uses Generative AI and AWS services to automate contract analysis.

Key points to know:

* Previously, AArete used a rule-based approach to process contracts, achieving approximately **55% accuracy** between 2020 and 2023. After moving to an AI-based solution on AWS in 2024, Doczy.ai achieved approximately **99% accuracy**.
* Doczy.ai uses a technique called **Smart Chunking** to preserve the structure, context, and relationships between different sections of a contract instead of simply splitting a document into fixed-size text chunks.
* The system combines **semantic analysis** to understand the meaning of the content with **structural analysis** to identify the structure and organization of the document.
* **Amazon Cognito** supports user authentication, while **Amazon S3** is used to store documents.
* **AWS Lambda** triggers the document processing workflow, and **Amazon Textract** extracts content from the documents.
* Subsequent workloads are processed using **Amazon ECS**, while **Amazon Bedrock** provides Generative AI capabilities.
* The processed results are sent to **Snowflake** for data analysis, while **AWS Secrets Manager** and **Amazon CloudWatch** support security, secrets management, and system monitoring.
* Over approximately 22 months, Doczy.ai processed **2.5 million documents**, equivalent to around **50 million pages**, made **137 million API calls** to Amazon Bedrock, and processed approximately **442 billion tokens**.
* The platform can process up to **250,000 documents per week**, achieve approximately **99% accuracy**, and reduce **manual processing time by 97%**.
* According to AWS, the solution has generated approximately **$330 million in direct and indirect savings** for customers.

One of the most interesting aspects of Doczy.ai is that Generative AI does not operate independently. Amazon Bedrock is only one component of a complete pipeline that includes authentication, storage, document processing, compute, AI, data, security, and monitoring.

This use case demonstrates that Generative AI is not limited to building chatbots or generating content. When combined with the right architecture, **Amazon Bedrock, Amazon Textract, and other AWS services can transform millions of pages of unstructured documents into useful and actionable enterprise data.**

![Architecture of Doczy.ai](/images/3-Blogs%20posted/Architecture%20of%20Doczy.ai.png)

*Architecture of Doczy.ai on AWS*

[Automating contract intelligence with Doczy.ai™ on AWS](https://aws.amazon.com/blogs/architecture/automating-contract-intelligence-with-doczy-ai-on-aws/)
