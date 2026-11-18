# Lab: Connect Agents in Copilot Studio

## Overview
In this hands-on lab, you will connect the existing **Expense Claims Agent** to the **Real Estate Booking Service** agent. You will configure the booking agent to delegate employee expense-policy questions to the connected agent, then test how the agents work together.

**Prerequisites:**
- A **Real Estate Booking Service** agent and an **Expense Claims Agent** in the same Copilot Studio environment
- The **Expenses_Policy.docx** document added to the Expense Claims Agent as a knowledge source, with status **Ready**
- Permission to edit the booking agent and to use and publish the Expense Claims Agent
- Generative orchestration enabled for the booking agent

---

## Lab Architecture
By the end of this lab, you will have:
- The **Real Estate Booking Service** as the primary agent
- The existing **Expense Claims Agent** connected as a separate, published agent
- Expense-policy questions delegated to the connected agent
- Real estate questions handled by the booking agent's existing capabilities
- A tested interaction verified in the activity map

---

## Step 1: Verify the Existing Agents

In this step, you will confirm that the expense agent can answer questions and is available to connect.

### 1.1 Verify the Expense Claims Agent
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and select the appropriate environment
2. Select **Agents** in the left navigation pane and open **Expense Claims Agent**
3. On the **Knowledge** tab, verify that **Expenses_Policy.docx** has status **Ready**
4. Open **Settings** and select the **Generative AI** tab. Verify that **Let other agents connect to and use this one** is enabled. If it is off, enable it and save if prompted.
5. In the **Test your agent** pane, start a new test session and ask:
   ```text
   According to Contoso's expense policy, what is the nightly hotel limit and what kind of hotel should I choose for a business trip?
   ```
6. Verify the answer against the policy. The provided policy specifies **$150 per night** and a standard or moderate hotel. Check for a citation to the policy.
7. Verify that the latest version of the agent is published. If you made changes or have not published the current version, select **Publish**, then **Publish** again.

> **Note:** The booking agent uses the published version of the connected agent, not its unpublished draft. Publishing also releases other saved changes to the agent's existing channels. Review its authentication and sharing settings before publishing or sharing it.

### 1.2 Verify the Booking Agent
1. Return to the **Agents** page and open **Real Estate Booking Service**
2. In **Settings** > **Generative AI**, verify that **Generative orchestration** is enabled. If it is off, enable it and select **Save**.
3. Close **Settings**

> **Note:** Changing orchestration can affect how existing topics are selected. If you enable it, retest the booking agent's existing workflows.

---

## Step 2: Connect the Expense Claims Agent

In this step, you will add the existing expense agent to the booking agent. You do not need to create another agent or copy its knowledge source.

1. On the **Real Estate Booking Service** agent, select the **Agents** tab
2. Select **+ Add**.
3. Under **Select an agent in your environment**, select **Expense Claims Agent**
4. Review the agent's details and update its description for the booking agent:
   ```text
   Answer questions about expense policies and submitting expense claims, including eligible expenses, accommodation, receipts, and reimbursement guidelines.
   ```
5. If **Pass conversation history to this agent** is available, clear it for this test
6. Select **Add and configure**, then save if prompted
7. Verify that **Expense Claims Agent** appears on the **Agents** tab and is enabled

> **Note:** Clearing conversation history limits what is passed to the explicit delegated task. This setting remains off until you change it, so include the necessary context in each expense question. Do not enter sensitive personal information.

> **Note:** The description is local to this connection. Changes to the expense agent's own description do not automatically update it here.

---

## Step 3: Update the Booking Agent Instructions

In this step, you will guide the booking agent to use the connected agent for expense questions while retaining its real estate capabilities.

1. On the **Overview** tab, select **Edit** in the **Instructions** section
2. Append the following to the existing instructions:
   ```text
   For questions about expense policies or how to submit expense claims, use [Expense Claims Agent]. Base the answer on the connected agent's response and do not invent policy details.

   Continue to handle real estate property and booking requests using the existing topics, tools, and knowledge sources.
   ```
3. Replace `[Expense Claims Agent]` with an agent reference by typing `/` and selecting **Expense Claims Agent**. If the reference picker is unavailable, use the agent's name instead.
4. Select **Save** if prompted

> **Note:** Keep the booking agent's existing knowledge sources and instructions. The connected agent uses its own published configuration and knowledge; adding it does not require removing the booking agent's real estate sources. Instructions and descriptions guide routing but do not guarantee it.

---

## Step 4: Test the Connected Agents

### 4.1 Test an Expense-Policy Question
1. Open the **Test your agent** pane for **Real Estate Booking Service** and start a new test session
2. Ask:
   ```text
   According to Contoso's expense policy, what is the nightly hotel limit and what kind of hotel should I choose for a business trip?
   ```
3. Review the **Activity** tab or activity map and verify that **Expense Claims Agent** was invoked
4. Compare the answer with the Expenses Policy and the response from your direct test. With the provided policy, verify that it states **$150 per night** and describes a standard or moderate hotel.

> **Note:** A plausible answer alone does not prove that the connected agent was used. Citations are not always retained when an answer is passed back to the primary agent, so use the activity map and the policy to verify the result.

### 4.2 Test a Real Estate Question
1. Start a new test session
2. Ask a question about a property in your existing knowledge source. For example, if your list contains **555 Oak Lane**, ask:
   ```text
   What is the asking price of 555 Oak Lane?
   ```
3. Verify in the activity map that the expense agent was not invoked
4. If the agent provides a price, compare it with the current property data. If the information is unavailable, verify that the agent says so rather than guessing.

---

## Verification Checklist
- [ ] Expense Claims Agent's policy knowledge source is **Ready**
- [ ] Expense Claims Agent answers a policy question and its latest version is published
- [ ] Expense Claims Agent allows connections from other agents
- [ ] Generative orchestration is enabled for Real Estate Booking Service
- [ ] Existing Expense Claims Agent added and enabled as a connected agent
- [ ] Connected agent description and booking agent instructions identify expense questions
- [ ] Expense question invokes the connected agent in the activity map
- [ ] Answer checked against the Expenses Policy
- [ ] Real estate question does not invoke the expense agent
- [ ] Existing real estate knowledge sources and capabilities retained

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Verified the existing expense agent and its published knowledge
2. ✅ Connected it to the real estate booking agent
3. ✅ Updated the description and instructions to guide delegation
4. ✅ Tested an expense question and verified the connected agent was invoked
5. ✅ Checked that a real estate question remained with the booking agent

### Key Learnings
- **Connected agents:** Reuse an existing agent without creating another agent or copying its knowledge
- **Published versions:** The primary agent calls the connected agent's published version
- **Descriptions and instructions:** Help the orchestrator decide which agent should handle a request
- **Conversation history:** Can be excluded from a connected-agent call when it is not needed
- **Testing:** The activity map verifies delegation; compare generated answers with the source
- **Permissions:** Agent access, authentication, and sharing affect who can use the connection

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Connect Agents in Copilot Studio** lab.

You now have hands-on experience with:
- Connecting an existing agent
- Guiding requests to the appropriate agent
- Testing delegation and checking grounded answers
- Preserving the primary agent's existing capabilities

---

**End of Lab**
