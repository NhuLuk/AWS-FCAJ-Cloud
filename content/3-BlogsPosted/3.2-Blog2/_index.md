---

title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
----------------------

# BUILDING A MULTI-INTERFACE AI ASSISTANT WITH AMAZON Q BUSINESS AND SLACK ON AWS

In enterprises, information is often scattered across multiple sources, including internal documents, guidelines, company policies, and conversations on Slack. When employees need specific information, they may have to search through multiple documents or browse large numbers of old messages.

While exploring real-world applications of Generative AI on AWS, I came across an interesting solution: building a **multi-interface AI Assistant with Amazon Q Business and Slack**, allowing users to access internal organizational knowledge directly from the tools they already use.

Key points to know:

* Users can ask questions through **Amazon Q Business** or directly within **Slack**, while both interfaces can access the organization's knowledge sources.
* The system uses **Retrieval Augmented Generation (RAG)**. Instead of relying only on the AI model's existing knowledge, the system retrieves relevant information from enterprise data and uses it as context to generate responses.
* **Amazon S3** is used to store documents and data used by the system.
* **Amazon Kendra** supports searching and retrieving relevant information from the organization's knowledge sources.
* Users can interact with the AI Assistant through multiple interfaces, including **Amazon Q Business** and **Slack**.
* Conversation data from Slack can be processed using **AWS Lambda** and automatically added to the knowledge source on a schedule using **Amazon EventBridge**.
* The solution supports **clickable references**, allowing users to access the source documents used by the AI when generating an answer.
* **Amazon CloudFront** helps distribute documents stored in Amazon S3 so that references can be conveniently accessed by users.
* **Amazon CloudWatch** is used to monitor the operation and performance of the system after deployment.
* **User feedback** can also be collected to evaluate and continuously improve the quality of the AI Assistant's responses.

One of the most interesting aspects of this architecture is that it is not simply about building another chatbot. Instead, the idea is to **bring AI to where users already work rather than requiring users to go to a separate AI interface**.

The AI Assistant can use internal enterprise data, operate across multiple interfaces, and, importantly, provide references that allow users to verify the sources behind generated answers.

This demonstrates how combining **Amazon Q Business, Slack, Amazon Kendra, Amazon S3, and Amazon CloudFront** can integrate Generative AI directly into enterprise workflows, making internal knowledge easier and faster to find and access.

![Multi-interface AI Assistant Architecture](/images/3-Blogs%20posted/AI%20Assistant.png)

*Multi-interface AI Assistant architecture with Amazon Q Business and Slack on AWS*

[Build a multi-interface AI assistant using Amazon Q and Slack with Amazon CloudFront clickable references from an Amazon S3 bucket](https://aws.amazon.com/blogs/machine-learning/build-a-multi-interface-ai-assistant-using-amazon-q-and-slack-with-amazon-cloudfront-clickable-references-from-an-amazon-s3-bucket/)
