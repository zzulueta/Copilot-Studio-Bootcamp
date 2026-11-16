# Lab: Ground Agent Responses with Microsoft Lists

## Overview
In this hands-on lab, you will configure the **Real Estate Booking Service** agent to answer questions using approved knowledge sources. You will enable Microsoft authentication and generative AI orchestration, add the **Real Estate Properties** and **Real Estate Booking Requests** Microsoft Lists as knowledge sources, update the fallback topic, and test the agent's responses and booking flow.

**Prerequisites:**
- A **Real Estate Booking Service** agent in your Copilot Studio environment
- The **Real Estate Properties** Microsoft List containing the sample properties
- The **Real Estate Booking Requests** Microsoft List
- The **Get Property** and **Create Booking Request** agent flows and the **Book a Real Estate Showing** topic
- The real estate website knowledge source already added to the agent

---

## Lab Architecture
By the end of this lab, you will have:
- Enabled Microsoft authentication and generative AI orchestration
- Added both Microsoft Lists as knowledge sources for generative answers
- Restricted generative answers to the approved lists and real estate website
- Configured the system fallback topic with a grounded answer and a controlled response
- Tested list-based answers, the showing topic, and the booking request flow

> **Note:** Generative answers use the Microsoft Lists as knowledge sources. The **Get Property** and **Create Booking Request** agent flows remain available as tools for structured lookup and booking actions.

---

## Step 1: Configure Microsoft Authentication

In this step, you will configure the agent to authenticate users with their Microsoft accounts so it can access the SharePoint lists.

1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment.
2. Open the **Real Estate Booking Service** agent.
3. Select **Settings** in the upper-right corner.
4. Select the **Security** tab, then select **Authentication**.
5. Select **Authenticate with Microsoft**.
6. Select **Save**, then select **Save** again in the confirmation window.

---

## Step 2: Ground Generative Answers in Approved Sources

In this step, you will enable generative AI orchestration and configure answers to use the website and Microsoft Lists rather than general model knowledge.

### 2.1 Enable Generative AI Orchestration
1. In the **Generative AI** section, turn **Orchestration** on.
2. Select **Save**, then close Settings.

### 2.2 Add the Microsoft Lists as Knowledge Sources
1. Select the **Knowledge** tab, then select **+ Add knowledge**.
2. In the **Upload file** section, select **SharePoint**.
3. Select **Browse items**.
4. Select the **Real Estate Properties** list and the **Real Estate Booking Requests** list.
   - If a list is not shown, open it in SharePoint, then return to the knowledge source picker and check **Recent Lists**.
   - You can also enter the list URL to select a list directly.
5. Select **Confirm selection**.
6. If prompted, enter a descriptive name and description for each list:
   - **Real Estate Properties:** `Use this list to answer questions about available properties, including property names, locations, asking prices, and bedroom counts.`
   - **Real Estate Booking Requests:** `Use this list to answer questions about saved showing requests.`
7. Select **Add to agent**.

> **Note:** Users need read access to a list to receive answers grounded in its contents. The booking-request list may contain customer details, so only grant access to users who are authorized to see those requests.

### 2.3 Configure Conversational Boosting
1. Select the **Topics** tab and select the **System** filter.
2. Open the **Conversational boosting** topic.
3. Select the **Create generative answers** node, then select **Edit data sources**.
4. Review the available options:
   - **Knowledge sources:** Search only selected sources
   - **Web Search**
   - **Allow the AI to use its own general knowledge**
   - **Content moderation level**
5. Select **Search only selected sources**.
6. Select **Add knowledge** and choose the website, **Real Estate Properties**, and **Real Estate Booking Requests** knowledge sources.
7. Disable **Allow the AI to use its own general knowledge**.
8. Select **Save**.

---

## Step 3: Configure the Fallback Topic

In this step, you will use the same approved sources in the system fallback topic and add a clear response for requests the agent cannot answer.

### 3.1 Add Grounded Generative Answers
1. In the **Topics** tab, select the **System** filter and open the **Fallback** topic.
2. On the existing **Message** node, select the ellipsis **…** and select **Delete**.
3. Select the **+** icon under the **Condition** node, select **Advanced**, then select **Generative answers**.
4. In the **Input** field, select the ellipsis **…**, open the **System** tab, and select **Activity.Text**.
5. Select **Edit** under **Data sources**.
6. Select **Search only selected sources**.
7. Select the website, **Real Estate Properties**, and **Real Estate Booking Requests** knowledge sources.
8. Disable **Allow the AI to use its own general knowledge**.
9. Under **Content moderation level**, select **Customize** and choose **Medium**.
10. Select **Save**.

### 3.2 Add a Controlled Fallback Response
1. In the **Fallback** topic, add a **Send a message** node after the **Create generative answers** node.
2. Enter:
   ```text
   Sorry, I don’t have enough information to help with that request. I can assist with booking real estate showings or questions related to available properties.
   ```
3. Select **Save**.

> **Note:** The system fallback topic is for requests where the agent cannot confidently select a topic or generative answer path. The message communicates the agent's scope when a grounded response is unavailable.

---

## Step 4: Test Generative Answers and Booking

In this step, you will check that the agent can answer from the approved sources, handle a booking through its topic, and return a controlled response when it cannot find relevant information.

### 4.1 Test Property Answers from the Microsoft List
1. Open the **Test your agent** pane.
2. Start a new test session and enable **Track between topics** if it is not already enabled.
3. Ask:
   ```text
   What is the asking price, street, and city of 555 Oak Lane?
   ```
4. Verify that the response is grounded in the **Real Estate Properties** list.
5. To check a property question through generative answers rather than the flow, temporarily turn off the **Get Property** tool in the **Tools** tab.
6. Ask:
   ```text
   What properties have 3 bedrooms?
   ```
7. Verify that the agent answers from the **Real Estate Properties** list.
8. Turn the **Get Property** tool back on in the **Tools** tab.

### 4.2 Test the Showing and Booking Request
1. In the test pane, start a new test session.
2. Enter:
   ```text
   I want to book a real estate showing
   ```
3. Enter a fictitious name and email address, then select **Yes** to confirm the details.
4. Select a property type and enter `3` for the number of bedrooms.
5. Verify that the agent displays **1100 High Villas**.
6. Enter a requested showing date and time when prompted.
7. Verify that the agent displays a booking confirmation.
8. Open the **Real Estate Booking Requests** list and verify that the request was added.

### 4.3 Test the Controlled Fallback
1. In the test pane, start a new test session.
2. Ask an unrelated question that is not covered by the website or either list, such as:
   ```text
   What is the population of Mars?
   ```
3. Verify that the agent responds with grounded information if available, or communicates its scope and limitations instead of guessing.

---

## Verification Checklist
- [ ] Microsoft authentication is enabled and the agent is published
- [ ] Generative AI orchestration is enabled
- [ ] The **Real Estate Properties** and **Real Estate Booking Requests** lists are added as knowledge sources
- [ ] The website and both Microsoft Lists are selected for conversational boosting
- [ ] General model knowledge is disabled for grounded generative answers
- [ ] The fallback topic uses the approved sources and includes a controlled response
- [ ] The agent answers property questions using the **Real Estate Properties** list
- [ ] The showing conversation completes and the booking request appears in the requests list

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Configured Microsoft authentication for SharePoint access
2. ✅ Enabled generative AI orchestration
3. ✅ Added Microsoft Lists as knowledge sources
4. ✅ Restricted generative answers to approved lists and a website
5. ✅ Updated the system fallback topic with grounded answers and a controlled response
6. ✅ Tested property answers and the end-to-end booking experience

### Key Learnings
- **Authentication:** Microsoft authentication lets the agent access SharePoint content on behalf of the user
- **Microsoft Lists:** Can serve as knowledge sources for structured property and booking data
- **Knowledge grounding:** Restricting sources helps keep answers relevant to approved business information
- **Generative AI orchestration:** Helps the agent select among topics, tools, and generative answers
- **Fallback topics:** Provide a controlled path when the agent cannot confidently respond
- **Agent flows:** Support structured tasks such as looking up a property or saving a booking request
- **Testing:** Validate both generated answers and data-changing actions in the test pane and the Microsoft Lists

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Ground Agent Responses with Microsoft Lists** lab.

You now have hands-on experience with:
- Configuring Microsoft authentication
- Grounding generative answers in Microsoft Lists and a website
- Restricting generative answers to selected sources
- Configuring and testing the fallback topic
- Testing agent flows and booking requests end to end

---

**End of Lab**
