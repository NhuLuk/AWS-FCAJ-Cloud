---
title: "AgentForge Day 2"
date: 2026-08-08
weight: 1
chapter: false
pre: "<b>4.1. </b>"
---

# Event Report: “AgentForge Day 2 – Advanced Amazon Bedrock AgentCore”

**Date:** August 8, 2026  
**Topic:** Personalization, Evaluation & Optimization  
**Main Content:** Advanced Amazon Bedrock AgentCore

---

### Event Overview

On **August 8, 2026**, I had the opportunity to participate in **AgentForge Day 2**, with the topic **Personalization, Evaluation & Optimization**. The event gave me an opportunity to explore additional knowledge related to building and operating AI agents on AWS.

The main content of the event focused on **Advanced Amazon Bedrock AgentCore**, particularly capabilities that support AI agents in maintaining context, monitoring their activities, evaluating performance, and optimizing their behavior.

The event consisted of two main sessions:

- An **Amazon Bedrock AgentCore** session from **09:00 – 10:00**.
- A **Hands-on Lab** from **10:00 – 11:00**, focusing on exploring and practicing several AgentCore capabilities.

During the first session, participants were introduced to advanced AgentCore topics such as **Memory, Evaluations, and Observability**. The session also covered several other components and concepts, including **Registry, Harness, Tools, Payments, Optimization, and Policy**.

The Hands-on Lab provided participants with a more practical opportunity to explore some of the topics introduced during the presentation, including adding Memory for personalized agent behavior, exploring Agent Observability, using AgentCore Evaluations to measure agent performance, and exploring AgentCore Harness.

Through this event, I gained a better understanding that building an AI agent involves more than receiving requests and generating responses. When an agent is used in real-world applications, developers also need to consider contextual information, monitoring, performance evaluation, and continuous optimization.

![Participating in AgentForge Day 2](/images/4-EventParticipated/4.1-Event1/agentforge-day2.png)

*Figure 1. Participating in AgentForge Day 2 on August 8, 2026.*

---

### Main Event Content

#### Amazon Bedrock AgentCore

The first part of the event introduced advanced topics related to **Amazon Bedrock AgentCore**.

Three important capabilities were highlighted:

- **Memory**
- **Evaluations**
- **Observability**

These capabilities support different aspects of the AI agent lifecycle. Memory helps an agent maintain relevant information from interactions, Observability supports monitoring agent activities, while Evaluations provides mechanisms for assessing agent performance.

Through this session, I gained a clearer understanding of a more complete AI agent development process. Instead of focusing only on building the initial agent logic, developers also need to consider how the agent will be monitored, evaluated, and improved after deployment.

---

#### AgentCore Memory

One of the topics I found particularly interesting during the event was **AgentCore Memory**.

Memory helps an agent maintain and use information from interactions to provide a more personalized experience. Instead of processing every request as a completely independent interaction, an agent can use relevant contextual information to provide responses that better match the user's situation.

During the Hands-on Lab, participants explored how to:

- Add Memory to an agent.
- Use Memory to support personalized agent behavior.
- Observe how contextual information can influence agent interactions.

Through this section, I gained a better understanding of the role of Memory in AI agent applications that may interact with users across multiple steps or sessions.

Memory also helped me recognize the difference between a system that simply processes individual questions and an agent that can use information from previous interactions to provide a more relevant user experience.

---

#### AgentCore Evaluations

Another important topic introduced during the event was **AgentCore Evaluations**.

When developing an AI agent, the ability to generate a response alone is not enough to determine whether the system is performing effectively. Developers also need methods to evaluate the quality and performance of the agent across different use cases.

AgentCore Evaluations supports this process by providing mechanisms that can help developers understand how an agent is performing and identify areas that may require further improvement.

During the Hands-on Lab, participants explored how **AgentCore Evaluations** can be used to measure agent performance.

Through this topic, I learned that AI agent development can be considered a continuous process:

```text
Build Agent
     ↓
Run Agent
     ↓
Observe Behavior
     ↓
Evaluate Performance
     ↓
Optimize Agent