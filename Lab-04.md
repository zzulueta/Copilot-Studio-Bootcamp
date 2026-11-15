# Lab: Manage Nodes in Copilot Studio

## Overview
In this hands-on lab, you will extend the **Real Estate Booking Service** agent by making its customer-details topic reusable and building a showing-request conversation with nodes. You will test both confirmation paths, publish the agent, and try it on the demo website.

**Prerequisites:**
- A **Real Estate Booking Service** agent in your Copilot Studio environment, configured to use classic orchestration
- A **Customer Details** topic that collects a name and email address, displays an Adaptive Card summary, and asks whether the details are correct using **Yes** and **No**
- A **Book a Real Estate Showing** topic with trigger phrases, including `I want to book a real estate showing`
- Permission to change the agent's authentication settings and publish it for a demo

---

## Lab Architecture
By the end of this lab, you will have:
- A reusable **Customer Details** topic that returns its collected values to the calling topic
- A showing-request topic with message, redirect, condition, and question nodes
- A flow that asks users to re-enter their details when they select **No**
- A published agent tested through the demo website

> **Note:** This lab collects details in a conversation only. It does not connect to a Microsoft List, save a booking request, or schedule a showing.

---

## Step 1: Configure Topic Variable Outputs

In this step, you will make the customer-details variables available to a topic that calls **Customer Details**.

### 1.1 Open the Customer Details Topic
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment
2. Select **Agents** in the left navigation pane
3. Select the **Real Estate Booking Service** agent
4. Select the **Topics** tab
5. Select the **Customer Details** topic

### 1.2 Return Variables to the Calling Topic
1. Select **Variables** in the top bar to open the Variables pane
2. Under **Browse**, select **Topic**
3. For each of the three variables containing the name, email address, and confirmation answer, select the checkbox for returning its value to other topics
4. Verify the confirmation variable is named `DetailsCorrect`. If Copilot generated a different name, select the variable and rename it.
5. Select **Save**

> **Note:** These are topic outputs, not global variables. When another topic redirects to **Customer Details**, it can use the returned values after the called topic finishes.

---

## Step 2: Build the Showing-Request Flow

In this step, you will add nodes to the **Book a Real Estate Showing** topic and use the confirmation answer to control the conversation.

### 2.1 Add a Message Node
1. Select the **Topics** tab and open **Book a Real Estate Showing**
2. Select the **+** icon under the **Trigger** node, then select **Send a message**
3. In **Enter a message**, enter:
   ```
   Hi, I can help you with booking a real estate property showing.
   ```
4. Select **Save**

### 2.2 Call the Customer Details Topic
1. Select the **+** icon under the message node
2. Select **Topic management** > **Go to another topic** > **Customer Details**
3. Check that the redirect node shows the returned name, email, and confirmation outputs
4. Select **Save**

> **Note:** After the called topic finishes, the conversation returns to **Book a Real Estate Showing**, where its output variables are available.

### 2.3 Add a Condition Node
1. Select the **+** icon under the redirect node and select **Add a condition**
2. Select the returned **DetailsCorrect** variable
3. Select **is equal to**, then select **Yes**
4. Select **Save**

### 2.4 Add Question Nodes
1. Select the **+** icon under the **Yes** condition branch and select **Ask a question**
2. Configure the question:
   - **Enter a message:** `Which property do you want to see?`
   - **Identify:** **User's entire response**
   - **Save user response as:** Select the variable and set its name to `PropertyName`
3. Select **Save**
4. Select the **+** icon under the new question node and select **Ask a question**
5. Configure the question:
   - **Enter a message:** `What date and time do you want to see the property?`
   - **Identify:** **Date and time**
   - **Save user response as:** Select the variable and set its name to `DateTime`
6. Select **Save**
7. Select the **+** icon under the date and time question node and select **Send a message**
8. In **Enter a message**, enter:
   ```
   Thank you. I have collected your showing details, but no booking has been scheduled.
   ```
9. Select **Save**

### 2.5 Test the Yes Path
1. If the **Test your agent** pane is closed, select **Test** in the upper-right corner
2. Select the ellipsis **…** at the top of the pane and enable **Track between topics** if it is not already enabled
3. Select the **Start new test session** icon
4. Enter the trigger phrase:
   ```
   I want to book a real estate showing
   ```
5. Enter a sample name and email address when prompted
6. Review the Adaptive Card and select **Yes**
7. For the property, enter:
   ```
   555 Oak Lane, Denver, CO 80203
   ```
8. For the date and time, enter:
   ```
   Tomorrow 10:00 AM
   ```
9. Verify the agent displays the final message and that the topic tracking shows the transition to **Customer Details** and back

---

## Step 3: Publish and Test the Agent

In this step, you will configure authentication for a demo, publish the agent, and test it on the demo website.

> **Important:** **No authentication** allows anyone with the demo link to interact with the agent. Use only a permitted training environment and fictitious sample data; do not enter real customer information. If your organization requires authentication, follow its policy instead of disabling it.

### 3.1 Configure Authentication
1. Select **Settings** in the upper-right corner of **Real Estate Booking Service**
2. Select **Security**, then **Authentication**
3. Select **No authentication**
4. Select **Save**, then select **Save** again in the confirmation window
5. Select **X** to close the Settings pane

### 3.2 Publish the Agent
1. Select **Publish** in the upper-right corner
2. Select **Publish** again to publish the current changes

### 3.3 Test the Demo Website
1. Select the ellipsis **…** next to **Settings** and select **Go to demo website**
2. In **Type your message**, enter:
   ```
   I want to book a property showing.
   ```
3. Answer the prompts using fictitious sample data and select **Yes** when asked to confirm the details
4. Verify that the agent asks for the property and requested date and time, then displays the final message

---

## Step 4: Handle the No Condition

The showing-request topic currently handles only the **Yes** path. In this step, you will redirect users back to the customer-details step when they select **No**.

### 4.1 Add the No-Path Redirect
1. Return to the **Book a Real Estate Showing** topic
2. Select the **+** icon under **All other conditions**
3. Select **Topic management** > **Go to step**
4. Select the earlier **Customer Details** redirect node
5. Verify that a redirect node appears under **All other conditions**
6. Select **Save**

### 4.2 Test the No Path
1. Open the **Test** pane and start a new test session
2. Enter:
   ```
   I want to book a real estate showing
   ```
3. Enter a sample name and email address
4. When asked whether the details are correct, select **No**
5. Verify that the agent returns to **Customer Details** and asks for the name again
6. Enter the corrected sample name and email address, select **Yes**, and complete the property and date prompts

### 4.3 Republish the Agent
1. Select **Publish**, then select **Publish** again to make the No-path changes available on the demo website
2. On the demo website, start a new conversation and repeat the No-path test

---

## Verification Checklist
- [ ] Name, email, and confirmation variables configured as topic outputs
- [ ] Showing-request topic calls **Customer Details** and uses the returned confirmation variable
- [ ] **Yes** branch collects a property and a date and time
- [ ] **No** branch returns to the **Customer Details** redirect
- [ ] Both paths tested in the Test pane
- [ ] Agent published and tested on the demo website
- [ ] Final changes republished and the No path tested on the demo website
- [ ] Test confirms that no booking is scheduled by this flow

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Configured variables to return values from a reusable topic
2. ✅ Added message, redirect, condition, and question nodes
3. ✅ Tested a showing-request flow across topics
4. ✅ Published the agent and tested it on the demo website
5. ✅ Added and tested a path for correcting customer details

### Key Learnings
- **Topic outputs:** Return collected values to a calling topic without making them global variables
- **Reusable topics:** Keep common conversation steps together and allow other topics to call them
- **Nodes:** Messages, questions, redirects, and conditions build a structured conversation
- **Variables:** Store responses for use in later steps and conditions
- **Testing:** Track between topics to verify transitions and test both confirmation paths
- **Authentication:** Demo links without authentication can be used by anyone who has the link
- **Publishing:** Saved topic changes must be published before they appear on the demo website
- **Booking limitations:** Collecting details in a conversation does not create or schedule a booking

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Manage Nodes in Copilot Studio** lab.

You now have hands-on experience with:
- Returning variables from reusable topics
- Creating topic flows with nodes and conditions
- Testing and correcting conversation paths
- Publishing and testing an agent on a demo website

---

**End of Lab**
