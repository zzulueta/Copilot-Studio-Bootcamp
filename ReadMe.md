# Microsoft Copilot Studio Bootcamp

A hands-on bootcamp for building, extending, and publishing agents with Microsoft Copilot Studio. You start with a simple knowledge-based agent, then build a **Real Estate Booking Service** agent step by step. You add topics, nodes, entities, agent flows, prompts, external APIs, MCP servers, event triggers, and multi-agent orchestration.

---

## Who This Course Is For
- Makers, business users, and developers who are new to Copilot Studio
- Power Platform and Microsoft 365 practitioners who want to build agents
- Anyone who wants to automate business processes with conversational AI

## Course Prerequisites
- A Microsoft 365 account with access to Microsoft Lists, SharePoint, Outlook, and Teams
- A Copilot Studio license, or the ability to sign up for a Copilot Studio free trial
- Access to a Power Platform environment where you can create solutions, agents, and agent flows
- Basic familiarity with a web browser and Microsoft 365

> **Note:** Each lab lists its own prerequisites. Most labs build on resources created in earlier labs, so complete them in order.

---

## Learning Objectives
By the end of this bootcamp, you will be able to:
- Create agents with natural language or manually, and keep them in a Power Platform solution
- Ground generative AI answers in documents, public websites, and Microsoft Lists
- Design conversations with topics, nodes, variables, Adaptive Cards, and entities
- Switch between classic and generative orchestration
- Automate actions with agent flows that read and write Microsoft Lists
- Extract structured data from files with prompts
- Call external REST APIs and reason over the responses
- Extend agents with Model Context Protocol (MCP) servers for Mail and Teams
- Run agents autonomously with event triggers
- Combine agents with connected agents and child agents
- Publish and test agents on the demo website

---

## Course Outline

### Module 1: Getting Started

| Lab | Title | What You Will Do |
|-----|-------|------------------|
| [Lab 01](./Lab-01.md) | Create Your First Agent with Copilot Studio | Set up a solution and publisher, create an **Expense Claims Agent** with natural language, manage topics, add a document knowledge source, and publish to the demo website. |
| [Lab 02](./Lab-02.md) | Build a Real Estate Agent with Copilot Studio | Create the **Real Estate Properties** Microsoft List, create the **Real Estate Booking Service** agent manually, and add a public website knowledge source. |

### Module 2: Conversation Design

| Lab | Title | What You Will Do |
|-----|-------|------------------|
| [Lab 03](./Lab-03.md) | Manage Topics in Copilot Studio | Disable an unused topic, enable classic orchestration, create a **Customer Details** topic with Copilot and an Adaptive Card summary, and create a **Book a Real Estate Showing** topic. |
| [Lab 04](./Lab-04.md) | Manage Nodes in Copilot Studio | Return topic output variables, build the showing conversation with message, redirect, condition, and question nodes, handle the **No** path, and publish the agent. |
| [Lab 05](./Lab-05.md) | Work with Entities in Copilot Studio | Review prebuilt entities, create a **Property Type** closed list entity and a **Number of Bedrooms** regular expression entity, and use them in the showing topic. |

### Module 3: Data, Automation, and Grounding

| Lab | Title | What You Will Do |
|-----|-------|------------------|
| [Lab 06](./Lab-06.md) | Create Agent Flows with Microsoft Lists | Create the **Real Estate Booking Requests** list, and build the **Get Property** and **Create Booking Request** agent flows. Call both flows from the showing topic. |
| [Lab 07](./Lab-07.md) | Ground Agent Responses with Microsoft Lists | Enable Microsoft authentication and generative orchestration, add both lists as knowledge sources, restrict answers to approved sources, and configure the fallback topic. |
| [Lab 08](./Lab-08.md) | Create Property Listings from Files with Prompts | Build a topic that accepts a listing file, extract property details as JSON with a prompt, and create a list item with an agent flow. |

### Module 4: Extending Agents

| Lab | Title | What You Will Do |
|-----|-------|------------------|
| [Lab 09](./Lab-09.md) | Call External APIs in Copilot Studio | Subscribe to an external API, send an HTTP request from a topic, parse the JSON response, and answer follow-up questions with a prompt. |
| [Lab 10](./Lab-10.md) | Use MCP Servers in Copilot Studio | Add the **Mail MCP** server to send, search for, and read email, and add the **Teams MCP** server to post a property update to a channel. |
| [Lab 11](./Lab-11.md) | Add Event Triggers to Copilot Studio Agents | Create an Outlook email trigger that saves booking requests to Microsoft Lists and sends an acknowledgment through the Mail MCP server. |
| [Lab 12](./Lab-12.md) | Add Connected and Child Agents in Copilot Studio | Connect the **Expense Claims Agent**, create a **Customer Support FAQ** child agent, and verify delegation in the activity map. |

---

## Lab Files
Sample files used in the labs are in the [Lab Files](./Lab%20Files) folder:

| Lab | File |
|-----|------|
| Lab 01 | [Expenses_Policy.docx](./Lab%20Files/Lab-01/Expenses_Policy.docx) |
| Lab 08 | [Listing 1.pdf](./Lab%20Files/Lab-08/Listing%201.pdf), [Listing 2.pdf](./Lab%20Files/Lab-08/Listing%202.pdf), [Listing 3.pdf](./Lab%20Files/Lab-08/Listing%203.pdf), [Listing 4.pdf](./Lab%20Files/Lab-08/Listing%204.pdf) |
| Lab 12 | [Contoso Support FAQ.docx](./Lab%20Files/Lab-12/Contoso%20Support%20FAQ.docx) |

---

## Important Notes
- Use a training environment and fictitious data. Some labs create real list items, send real email, and post Teams messages.
- Event triggers, agent flows, and generative answers can consume Copilot Credits.
- Lab 09 uses a third-party API. Review the provider's terms, plan limits, and charges before you subscribe.
- The Mail and Teams MCP servers used in Lab 10 are preview features. Availability, tool names, and permissions can change.

---

## Additional Resources
- [Microsoft Copilot Studio documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Copilot Studio training on Microsoft Learn](https://learn.microsoft.com/en-us/training/browse/?products=copilot-studio)
- [Copilot Studio licensing](https://learn.microsoft.com/en-us/microsoft-copilot-studio/billing-licensing)
