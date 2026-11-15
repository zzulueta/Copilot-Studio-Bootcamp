# Lab: Manage Topics in Copilot Studio

## Overview
In this hands-on lab, you will manage topics in the **Real Estate Booking Service** agent you created in Lab 02. You will disable an unused topic, switch to classic orchestration, create and refine a customer-details topic with Copilot, and create a topic for booking a real estate showing.

**Prerequisites:**
- Completion of **Lab 02: Build a Real Estate Agent with Copilot Studio**
- The **Real Estate Booking Service** agent created in your Lab 01 solution
- Access to the same Copilot Studio environment used in the previous labs

---

## Lab Architecture
By the end of this lab, you will have:
- Disabled the unused **Start Over** topic
- Configured the agent to use classic orchestration
- Created a **Customer Details** topic with Copilot
- Updated the topic to thank the user by name, display a summary Adaptive Card, and confirm the collected details
- Created a **Book a Real Estate Showing** topic with trigger phrases

---

## Step 1: Disable an Unused Topic

In this step, you will disable the **Start Over** topic to reduce ambiguity when the agent handles user requests.

### 1.1 Open the Agent Topics
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment
2. Select **Agents** in the left navigation pane
3. Select the **Real Estate Booking Service** agent
4. Select the **Topics** tab

### 1.2 Disable the Start Over Topic
1. Find the **Start Over** topic
2. Toggle **Enabled** to **Off**

> **Note:** Disabling unused topics helps reduce ambiguity when multiple topics or generative responses could handle the same request.

---

## Step 2: Enable Classic Orchestration

Classic orchestration uses trigger phrases to determine which topic should handle a user message.

### 2.1 Update the Orchestration Setting
1. Select **Settings** in the upper-right corner
2. On the **Generative AI** page, go to **Orchestration**
3. Select **No – Use classic orchestration**
4. Select **Save**
5. Select the **X** button to exit the Settings page

---

## Step 3: Create and Refine the Customer Details Topic

In this step, you will create a topic with Copilot, update its question node, and add an Adaptive Card and a confirmation question.

### 3.1 Create a Topic with Copilot
1. On the **Topics** page, select **+ Add a topic** > **Add from description with Copilot**
2. In the **Name your topic** text box, enter:
   ```
   Customer Details
   ```
3. In the **Create a topic to…** text box, enter:
   ```
   Ask the customer for their name and email address.
   ```
4. Select **Create**
5. Select **Save** and turn off the **Test** pane

> **Note:** The trigger contains a set of phrase examples. These are shown because classic orchestration is enabled.

### 3.2 Update the Email Question with Copilot
1. If the **Edit with Copilot** pane is not shown on the right, select the Copilot icon near the top of the authoring canvas
2. Select the second **Question** node, **What is your email address?**
3. In the **Edit with Copilot** pane, enter the following in **What do you want to do?**:
   ```
   Change "What is your email address?" to say thank you to the Name variable from the previous node and then proceed to ask the email address question.
   ```
4. Select **Update**
5. Verify the message thanks the user by using the name collected in the previous node, then asks for the email address
6. Select **Save**

> **Note:** If Copilot does not update the question correctly, select **Undo** and try again with a different prompt.

### 3.3 Add an Adaptive Card and Confirmation Question
1. Select an empty area of the authoring canvas so that no node is selected
2. Select the Copilot icon to open the **Edit with Copilot** pane
3. In **What do you want to do?**, enter:
   ```
   Summarize the information collected in an adaptive card
   ```
4. Select **Update**
5. Verify that a message node with an Adaptive Card is added to the end of the topic
6. Select the **Media** box in the Adaptive Card to open its properties
7. Verify the Adaptive Card formula. If it does not match, replace it with:
   ```powerfx
   {
       type: "AdaptiveCard",
       body:
       [
           {
               type: "TextBlock",
               size: "Medium",
               weight: "Bolder",
               text: "Summary"
           },
           {
               type: "FactSet",
               facts:
               [
                   {
                       title: "Full Name",
                       value: Text(Topic.Name)
                   },
                   {
                       title: "Email Address",
                       value: Text(Topic.EmailAddress)
                   }
               ]
           },
           {
               type: "TextBlock",
               text: "Thank you for providing the information."
           }
       ]
   }
   ```
8. Close the Adaptive Card properties pane and select the Copilot icon to reopen **Edit with Copilot**
9. Select an empty area of the authoring canvas so that no node is selected
10. In **What do you want to do?**, enter:
    ```
    Add a new multiple choice question to prompt the user if the details are correct with two options Yes or No
    ```
11. Select **Update**
12. Verify that a multiple-choice question node is added with **Yes** and **No** options
13. Select **Save**

### 3.4 Test the Customer Details Topic
1. If the **Test your agent** panel is closed, select **Test** in the upper-right corner
2. Select the **Start a new test session** icon at the top of the testing panel
3. In the **Ask a question or describe what you need** text box, enter:
   ```
   Customer information
   ```
4. Enter your name and email address when prompted
5. Review the summary card and select **Yes**

---

## Step 4: Create the Book a Real Estate Showing Topic

In this step, you will create a topic from blank and add example phrases that can trigger it. The topic will be expanded in later labs.

### 4.1 Create a Topic from Blank
1. Select the **Topics** tab at the top of the **Real Estate Booking Service** agent
2. Select **+ Add a topic** > **From blank**
3. Select the **Details** icon to open the Topic details dialog
4. Set the topic details:
   - **Name:** `Book a Real Estate Showing`
   - **Description:** `Select the property and requested date and create a booking request`
   - **Display name:** `Book Showing`
5. Select **Save**

### 4.2 Add Trigger Phrases
1. Under **Trigger**, select **Edit** for **User says a phrase**
2. Enter each phrase below under **Add phrases**, selecting the **+** icon after each one:
   - `I want to book a real estate showing`
   - `Schedule a real estate showing`
   - `Arrange the viewing for a real estate property`
   - `Set up an appointment to view a house`
   - `Plan a property viewing`
3. Select **Save**

---

## Verification Checklist
- [ ] **Start Over** topic disabled
- [ ] Classic orchestration enabled
- [ ] **Customer Details** topic created with Copilot
- [ ] Email question updated to thank the user by name
- [ ] Adaptive Card displays the collected name and email
- [ ] Confirmation question includes **Yes** and **No** options
- [ ] Customer Details topic tested successfully
- [ ] **Book a Real Estate Showing** topic created with the specified trigger phrases

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Disabled an unused topic
2. ✅ Enabled classic orchestration
3. ✅ Created a topic with Copilot from a natural-language description
4. ✅ Updated a question node and added an Adaptive Card and confirmation question
5. ✅ Tested the **Customer Details** topic
6. ✅ Created a **Book a Real Estate Showing** topic with trigger phrases

### Key Learnings
- **Topic management:** Disabling unused topics helps reduce ambiguity and improve how accurately the agent responds to user requests
- **Classic orchestration:** Trigger phrases provide more control over which topics match and handle user messages
- **Copilot topic creation:** Topics can be created quickly from natural-language descriptions
- **Trigger phrases:** Include varied phrases to improve topic recognition
- **Copilot-assisted editing:** Copilot can update existing nodes, use collected variables in messages, and add conversation steps
- **Adaptive Cards:** Present collected information in a structured, user-friendly format
- **Confirmation questions:** Allow users to validate their inputs and improve data accuracy
- **Testing:** Testing topics helps verify that their triggers, flow, and responses behave as expected
- **Manual topic authoring:** A topic's name, description, display name, and trigger phrases can be configured directly

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Manage Topics in Copilot Studio** lab.

You now have hands-on experience with:
- Managing topic settings and orchestration
- Creating and editing topics with Copilot
- Using variables in topic messages
- Adding Adaptive Cards and confirmation questions
- Creating manual topics with trigger phrases
- Testing topic flows in Copilot Studio

---

**End of Lab**
