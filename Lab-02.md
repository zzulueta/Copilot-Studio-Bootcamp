# Lab: Build a Real Estate Agent with Copilot Studio

## Overview
In this hands-on lab, you will create a Microsoft List to store real estate property data, then use Microsoft Copilot Studio to manually create a Real Estate Booking agent inside the solution you created. You will configure the agent's details and instructions, ground it in a public website knowledge source, and test its generative AI responses.

**Prerequisites:**
- Solution `Copilot Agent <FirstName> <LastName>` created.
- A Microsoft 365 account with access to Microsoft Lists and a SharePoint site
- A Copilot Studio license, or the ability to sign up for a Copilot Studio free trial

---

## Lab Architecture
By the end of this lab, you will have:
- A Microsoft List named **Real Estate Properties** containing sample property records
- A Copilot Studio agent named **Real Estate Booking Service** saved in your solution
- Agent details and instructions that guide booking-related conversations
- A public website knowledge source for grounded generative AI responses

---

## Step 1: Create the Real Estate Properties List

In this step, you will create a Microsoft List that stores real estate property data for the agent to work with.

### 1.1 Open Microsoft Lists
1. Open a new browser tab and sign in to [Microsoft Lists](https://www.microsoft.com/en-us/microsoft-365/microsoft-lists)

### 1.2 Create a Blank List
1. Select **+ Create new**
2. Select **Blank list**
3. Configure the list:
   - **Name:** `Real Estate Properties` (use your first and last name to make it unique)
4. Select **Create**

### 1.3 Add Columns
1. Select the **Title** column header > **Column settings** > **Rename**, and rename it to `Property Name`. Select **Save**.
2. Select **+ Add column** and add the following columns, one at a time. For each one, select the column type, select **Next**, enter the name, and select **Save**:

   | Column name | Column type |
   |---|---|
   | `Owner` | Person |
   | `Asking Price` | Currency |
   | `Street` | Text |
   | `City` | Text |
   | `Bedrooms` | Number |
   | `Bathrooms` | Number |

### 1.4 Add Test Data
1. Select **+ Add new item** to add a new item
2. Enter the following data:
   - **Property Name:** `1100 High Villas`
   - **Owner:** Select your user (search for your provided username)
   - **Asking Price:** `250,000`
   - **Street:** `Main Avenue`
   - **City:** `Redmond`
   - **Bedrooms:** `3`
   - **Bathrooms:** `2`
3. Select **Save**
4. Select **+ Add new item** and enter the following data:
   - **Property Name:** `555 Oak Lane`
   - **Owner:** Select your user
   - **Asking Price:** `300,000`
   - **Street:** `Oak Lane`
   - **City:** `Denver`
   - **Bedrooms:** `4`
   - **Bathrooms:** `3`
5. Select **Save**
6. **Result:** You now have 2 real estate properties in your list.

> **Note:** Copy the URL of your list and keep the browser tab open. You will use this list throughout the course.

---

## Step 2: Create an Agent Manually

In this step, you'll access Copilot Studio, select the appropriate environment, and create a new agent in the solution you created.

### 2.1 Open Copilot Studio
1. In a new browser tab, navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and sign in if prompted
2. Select the environment picker and make sure you are in the proper environment.
3. In the navigation pane on the left, select **Agents**
4. Select **New agent** > **Agent (Standard)** to create a new agent

### 2.2 Create the Agent in Your Solution
1. Name the agent `Real Estate Booking Service`
2. Open the **Agent settings** dropdown
3. Select the solution you created (`Copilot Agent <FirstName> <LastName>`)
4. Click **Create** to create the agent

> **Note:** Your agent will begin provisioning. Once it is ready, you can continue configuring it. Because it is saved in your solution, the solution now contains both the **Expense Claims Agent** and the **Real Estate Booking Service** agent.

### 2.3 Configure the Agent Details
1. In the **Details** section, select **Edit**
2. In the **Description** text box, enter:
   ```
   Create bookings for real estate properties
   ```
3. Select **Save**

### 2.4 Update the Instructions
1. Select the **Edit** icon in the **Instructions** section
2. Update the instructions to:
   ```
   You are a real estate booking assistant.

   Help users with questions related to real estate properties and booking showings by using the knowledge and data that are available to you.

   When responding:
   - Use the information provided through your configured knowledge sources whenever possible.
   - If a user's request is unclear or missing required details, ask a follow-up question to gather the information you need.
   - If you do not have enough information to answer confidently, do not guess. Instead, explain what information is missing or guide the user to provide it.
   - Keep responses clear, helpful, and focused on assisting the user with booking-related tasks.
   ```
3. Select **Save**

> **Note:** Agent instructions guide how the agent should behave, but they do not strictly enforce behavior. In later labs, you will learn how to make this behavior predictable by using topics, generative answers with restricted knowledge sources, and fallback configuration.

### 2.5 Test the Agent
1. In the **Test your agent** pane on the right, enter the following prompt:
   ```
   How do I make a booking?
   ```
2. Review the response. Without any knowledge sources, the answer is likely to be generic.
3. Leave the window open.

---

## Step 3: Add Generative AI Answers

In this step, you'll add a knowledge source that the agent can use to generate richer, grounded responses.

### 3.1 Add a Public Website Knowledge Source
1. Select the **Knowledge** tab
2. Select **+ Add knowledge**
3. Select **Public websites**
4. In the **Public website link** text box, enter:
   ```
   https://www.realtor.com/marketing/resources
   ```
   This public website has real estate marketing tips that could be useful for your agent.
5. Select **Add**
6. Select **Add to agent**

### 3.2 Test Generative AI Responses
1. Select the **Overview** tab
2. Select the ellipsis **…** menu at the top of the **Test your agent** pane
3. Enable **Track between topics**
4. At the top of the **Test your agent** pane, select the **Start new test session** icon (plus icon)
5. Enter the following prompt:
   ```
   How do I boost real estate promotion?
   ```
6. Verify that the agent responds with citations from the website

---

## Verification Checklist
- [ ] **Real Estate Properties** Microsoft List created in a SharePoint site
- [ ] List columns added (Property Name, Owner, Asking Price, Street, City, Bedrooms, Bathrooms)
- [ ] Two sample property records added to the list
- [ ] **Real Estate Booking Service** agent created in your solution
- [ ] Agent description and instructions updated
- [ ] Agent tested in the Test pane
- [ ] Public website knowledge source added
- [ ] Grounded responses with citations verified

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Created a Microsoft List to store real estate property data
2. ✅ Added custom columns and sample records to the list
3. ✅ Created an agent manually in Copilot Studio within your existing solution
4. ✅ Configured the agent's description and instructions
5. ✅ Grounded the agent with a public website knowledge source
6. ✅ Verified knowledge-grounded responses with citations

### Key Learnings
- **Environment alignment:** Working in the correct environment is essential to ensure all configurations, data, and agents are aligned and functional
- **Microsoft Lists:** Provide a quick, low-code way to store and manage structured business data, such as property records, without building a custom app
- **Solutions:** Creating agents inside a solution keeps related components together and makes them easier to manage and move between environments
- **Descriptions and instructions:** Clear agent descriptions and instructions guide how the agent responds, asks questions, and handles incomplete inputs
- **Instructions vs. enforcement:** Instructions influence behavior but do not strictly control it, highlighting the need for additional design elements for consistency
- **Knowledge sources:** Adding knowledge sources enables the agent to generate richer, more informed responses beyond its base configuration
- **Grounding:** Grounding responses in knowledge sources improves accuracy, relevance, and user trust
- **Testing:** Iterative testing using the built-in Test pane is critical to refining both conversational flow and response quality

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Build a Real Estate Agent with Copilot Studio** lab.

You now have hands-on experience with:
- Creating and configuring Microsoft Lists
- Creating agents manually in Copilot Studio within a solution
- Configuring agent descriptions and instructions
- Adding public website knowledge sources
- Tracking topics and testing generative AI responses in the Test pane

---

**End of Lab**
