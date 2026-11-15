# Lab: Create Your First Agent with Copilot Studio

## Overview
In this hands-on lab, you will set up a Power Platform solution, then use Microsoft Copilot Studio to create a simple agent that answers employee questions about expense policies in a fictional corporation (Contoso). You will create the agent using natural language, manage its topics, ground it in a knowledge source, and publish it to a demo website.

**Prerequisites:**
- A Microsoft 365 account
- A Copilot Studio license, or the ability to sign up for a Copilot Studio free trial
- Basic familiarity with a web browser and Microsoft 365

---

## Lab Architecture
By the end of this lab, you will have:
- A solution and publisher to hold your agent
- A Copilot Studio agent that helps employees with expense claims
- A custom topic and a document-based knowledge source for grounded generative AI responses
- A published agent available through the Demo website channel

---

## Step 1: Set Up the Solution

### 1.1 Sign In to Power Apps
1. Open a new browser tab and sign in to [Power Apps](https://make.powerapps.com/)
2. Select the environment picker and select your environment. This should be the environment assigned to you to create the solution.

### 1.2 Create a Solution
1. Select **Solutions** in the left navigation pane
2. Select **+ New solution**
3. Configure the solution:
   - **Display name:** `Copilot Agent <FirstName> <LastName>` (use your first and last name to make it unique)
4. Select **+ New publisher** and configure it:
   - **Display name:** Your name
   - **Name:** Your name with **no spaces**
   - **Prefix:** A short prefix (preferably your initials)
   - Click **Save**
5. Click **Create** to create the new solution
6. **Result:** You have an empty solution in your assigned environment.

---

## Step 2: Create an Agent Using Natural Language

In this step, you'll use Copilot Studio to create a new agent. The agent will initially have very limited capabilities, which you'll extend later in the lab.

### 2.1 Open Copilot Studio
1. In a web browser, navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and sign in if prompted
2. Select the environment picker and make sure you are in the correct environment.
3. In the navigation pane on the left, select **Agents**. This page shows all the agents you have created previously.
4. Select **New agent** > **Agent (Standard)** to create a new agent.

> **Note:** You can create a completely new agent or start with a template. In this lab, you'll create a brand-new agent.

### 2.2 Create the Agent
1. Name the agent `Expense Claims Agent`.
2. Open the **Agent settings** dropdown.
3. Select the solution you created earlier.
4. Click **Create** to create the agent.

### 2.3 Review the Agent
1. Review the agent created by Copilot Studio:
   - Check the **Details** of the agent
   - Review the **Model** selected (the model may vary based on your subscription)
   - Review the **Instructions**

### 2.4 Update the Instructions
1. Select the **Edit** icon in the **Instructions** section
2. Change the instructions so your agent:
   ```
   # Role
   You are the Expense Claims Agent for Contoso. You help Contoso employees understand the company's expense policy and how to submit expense claims.

   # Tone and style
   - Keep a friendly and professional tone at all times.
   - Keep answers clear and short. Use bullet points or short numbered steps for lists, limits, and processes.
   - Use plain language. Explain any finance terms you use.
   - If a question is vague, ask one short question to clarify before you answer. For example: "Is this for domestic or international travel?"

   # Knowledge and grounding
   - Base answers on the Contoso Expenses Policy document in your knowledge sources.
   - When you give a limit, rule, or requirement, quote it as the policy states it and include a citation.
   - If the policy doesn't cover the question, say so plainly. Don't guess, and don't use general internet or industry norms in place of Contoso policy.
   - Never make up expense limits, approval thresholds, deadlines, reimbursement amounts, or contact details.

   # What you can help with
   - Expense limits and eligible expenses, such as flights, hotels, meals, mileage, and entertainment.
   - Which receipts and documents are needed.
   - How to prepare and submit an expense claim.
   - Explaining policy rules, exceptions, and what isn't reimbursable.

   # Boundaries
   - Avoid providing any tax advice. This includes tax deductibility, tax filing, VAT/GST reclaim strategies, and personal tax implications. Politely decline and suggest the user consult a qualified tax professional.
   - Don't give legal or investment advice.
   - Don't approve, reject, or promise reimbursement for any claim. Explain only what the policy says.
   - Don't ask for or store sensitive personal data, such as bank account numbers, card numbers, or passwords. If a user shares this kind of data, remind them not to.
   - If a question isn't about Contoso expenses, politely say you can only help with expense-related questions.

   # Response format
   1. Answer the question directly first.
   2. Add relevant details or conditions from the policy.
   3. Include a citation to the Expenses Policy.
   4. If it helps, finish with a short next step, such as "Attach itemized receipts when you submit your claim."
   ```
3. Select **Save**

### 2.5 Test the Agent
1. In the **Test your agent** pane on the right, enter the following prompt:
   ```
   Hello
   ```
2. Review the response, which should be an appropriate greeting. Notice that the **Greeting** topic was used.
3. Try the following prompt:
   ```
   Who should I contact about submitting an expense claim?
   ```
   The response may be appropriate, but it's likely to be generic. In a real organization, you'd want the agent to provide an email address or phone number for the user to contact.
4. Try another prompt:
   ```
   What's the expense limit for a hotel stay?
   ```
   Again, the response may be appropriate but generic. In a real organization, you'd want a more specific response based on the company's expense policies.
5. Try another prompt:
   ```
   How do I pay taxes?
   ```
   The agent should decline to answer, since you specified this in the instructions.
6. Close the **Test your agent** pane

---

## Step 3: Manage Topics in Your Agent

You can use topics to provide explicit responses to triggers, such as common questions or requests that you expect your users to enter.

### 3.1 Review the Existing Topics
1. In the page for your agent, select the **Topics** tab
   - **Custom** topics are triggered by input from the user
   - **System** topics are triggered by specific events, such as errors or unexpected input
   - You can filter topics by category, or use the **All** filter to see them all
2. Select the **Greeting** custom topic to view it on the authoring canvas (a visual designer for creating and editing topics)
3. Note that the **Greeting** topic is triggered by an input containing any of the following phrases:
   - Good afternoon
   - Good morning
   - Hello
   - Hey
   - Hi

   The response to this trigger is the message *"Hello. How can I help you today?"* This explains the response you saw when testing the agent.
4. Return to the **Topics** page and view the **System** topics. Note the following:
   - **Conversational boosting:** Triggered when the user submits a message for which the agent can't identify a corresponding topic (the user's intent is unknown). The topic then attempts to respond using generative AI.
   - **Fallback:** A "fail-safe" topic that responds when the intent is unknown and an appropriate generative AI response can't be produced. It lets the user try again up to three times before gracefully ending the conversation, often by escalating to a human operator.

### 3.2 Create a Topic with Copilot
1. On the **Topics** page, in the **+ Add a topic** menu, select **Topic** > **Add from description with Copilot**
> If prompted, select **Allow** to let the browser see text and images copied to the clipboard
2. In the **Add from description with Copilot** dialog, configure the topic:
   - **Name:** `Ask about expenses contact`
   - **Description:**
     ```
     When the user asks who to contact about expense claims, tell them to send an email to finance@contoso.com
     ```
3. Select **Create**
4. After a short wait, a new topic named **Ask about expenses contact** is created and opened in the authoring canvas. It should be triggered by phrases asking about a contact for expenses, and respond with a message telling the user to email the appropriate address.

> **Important:** If the nodes in the topic don't match this description, delete the topic and create it again.

5. Select **Save** (top right) to save the new topic

### 3.3 Test the New Topic
1. Open the **Test** pane and enter the following prompt:
   ```
   Who should I contact about submitting an expense claim?
   ```
2. Verify that the response is based on the topic you just added. Even though the text doesn't exactly match any trigger phrase, it should be semantically close enough to trigger the topic.

---

## Step 4: Add a Knowledge Source for Generative AI Responses

You can add topics for all the inputs you expect, but you can't realistically anticipate every question. Currently, your agent uses the **Conversational boosting** topic to generate AI responses from a language model, which results in generic answers. You need to provide a knowledge source to ground generative AI responses in more relevant information.

### 4.1 Download the Expenses Policy
1. Download the **Expenses_Policy.docx** file found in the **Lab Files > Lab-01 folder** to download the expenses policy document locally. This document contains the expenses policy for the fictional Contoso corporation.

### 4.2 Upload the Knowledge Source
1. Return to the Copilot Studio tab and close the **Test your agent** pane
2. Select the **Knowledge** tab to see the knowledge sources defined in your agent (currently there should be none)
3. Select **+ Add knowledge** and note the multiple types of knowledge sources you can add
4. In the **Upload file** section, upload the expenses policy document you downloaded
5. Click **Add to agent**

> **Note:** After uploading, the file must be indexed, which may take 10 minutes or longer. The knowledge source is only available when its **Status** is **Ready**. Continue with the lab — you'll check on the indexing in Step 6.

---

## Step 5: Publish Your Agent

The channels through which you can deliver your agent depend on the type of authentication you configure. In this step, you'll enable access for anyone and publish the agent to a demo website.

### 5.1 Configure Authentication
1. At the top of the page, select the **Channels** tab and review the channels available. The available channels depend on your agent's authentication settings.
2. Select **Settings** at the top right of the page
3. In the **Settings** pane, on the **Security** page, select **Authentication**
4. Select **No authentication**, then click **Save**, and **Save** again to confirm you want to enable access to the agent for everyone
5. Close the **Settings** pane and view the **Channels** page

### 5.2 Publish the Agent
1. At the top of the page, select **Publish**
2. On the **Publish** page, select **Publish**. Publishing takes a minute or so.

### 5.3 Configure the Demo Website Channel
1. Select the **Demo website** channel. This is an appropriate channel for users to test your agent.
2. In the **Demo website** pane, configure the following:
   - **Welcome message:** `Ask me about Expense claims`
   - **Conversation starters:**
     ```
     Hello
     Who should I contact with expense enquiries?
     What are the expense limits for flights?
     ```
3. Select **Save**
4. Select **Copy** to copy the link to your agent's demo website

### 5.4 Test the Demo Website
1. In a new browser tab, navigate to the URL you copied to open the demo website
2. Enter the following message and view the response:
   ```
   What are the expense limits for meals?
   ```
3. Try a few more questions and the conversation starters. The agent has limited functionality, but should provide relevant answers about expense claims.
4. When you're done exploring, close the browser tab

---

## Step 6: Check the Knowledge Source Indexing

Check whether the file you uploaded has finished indexing. If not, take a coffee break and check back every few minutes.

### 6.1 Verify the Knowledge Source Status
1. Navigate back to the Copilot Studio tab and close the **Demo website** pane
2. Select the **Knowledge** tab
3. Check the **Status** of your uploaded file. If it is still **In progress**, refresh every few minutes until it is **Ready**.

### 6.2 Test Grounded Responses
1. Expand the **Test** pane and restart the conversation
2. Enter the following prompt:
   ```
   What's the expense limit for a hotel stay?
   ```
3. Verify that the response is based on the uploaded knowledge source and includes a citation reference
4. Try some follow-up questions:
   ```
   What about flights?
   ```
   ```
   What guidelines are there for entertainment expenses?
   ```

### 6.3 Republish the Agent
1. At the top of the page, select **Publish**
2. On the **Publish** page, select **Publish**

---

## Verification Checklist
- [ ] Power Platform developer environment available
- [ ] Solution and publisher created
- [ ] Expense claims agent created using natural language and saved to your solution
- [ ] Agent instructions updated
- [ ] Agent tested in the Test pane
- [ ] **Ask about expenses contact** topic created and tested
- [ ] Expenses policy document uploaded as a knowledge source with status **Ready**
- [ ] Authentication set to **No authentication**
- [ ] Agent published and Demo website channel configured
- [ ] Grounded responses with citations verified

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Set up a Power Platform developer environment and solution
2. ✅ Created an agent in Copilot Studio using natural language
3. ✅ Refined the agent's behavior through instructions
4. ✅ Reviewed built-in topics and created a custom topic with Copilot
5. ✅ Grounded the agent with an enterprise knowledge source
6. ✅ Published the agent to the Demo website channel
7. ✅ Verified knowledge-grounded responses with citations

### Key Learnings
- **Rapid creation:** Copilot Studio enables building agents quickly using natural language prompts to define their purpose and behavior
- **Iterative enhancement:** Agents start with limited capabilities and can be enhanced through topics, instructions, and knowledge sources
- **Environment and solution setup:** Selecting the right environment and solution (backed by Dataverse) are important steps before creating an agent
- **Instructions:** Define the agent's tone, boundaries, and behavior, including constraints such as avoiding certain topics (e.g., tax advice)
- **Built-in topics:** Topics like Greeting, Conversational boosting, and Fallback handle common interactions and unknown intents
- **Custom topics:** Define precise responses to specific user queries using triggers and structured flows, created manually or generated by Copilot from a description
- **Grounding:** Generative AI responses are more useful when grounded in enterprise knowledge sources rather than generic model knowledge
- **Indexing:** Knowledge sources (e.g., documents, SharePoint, websites) must be indexed before the agent can use them
- **Conversational boosting:** Uses both knowledge sources and the language model to answer questions when no topic matches
- **Testing:** The built-in Test pane is critical for validating agent responses and behavior before publishing
- **Publishing:** Makes the agent accessible through channels, with authentication settings controlling who can use it
- **User experience:** The Demo website channel, welcome messages, and conversation starters provide a simple way to share the agent and guide initial interactions
- **Continuous improvement:** Iteratively refining topics, instructions, and knowledge sources is key to improving accuracy and usefulness

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Create Your First Agent with Copilot Studio** lab.

You now have hands-on experience with:
- Power Platform developer environments and solutions
- Creating agents with natural language in Copilot Studio
- Configuring agent instructions
- Managing system and custom topics
- Adding knowledge sources for grounded generative AI responses
- Configuring authentication and publishing to channels
- Testing agents in the Test pane and on a demo website

---

**End of Lab**
