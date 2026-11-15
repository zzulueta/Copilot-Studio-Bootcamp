# Lab: Create Agent Flows with Microsoft Lists

## Overview
In this hands-on lab, you will create agent flows that connect the **Real Estate Booking Service** agent to Microsoft Lists. The agent will find a property by its number of bedrooms, then save a booking request in a separate list.

**Prerequisites:**
- A **Real Estate Booking Service** agent in your Copilot Studio environment
- A **Book a Real Estate Showing** topic that collects customer details, property preferences, and a requested showing date and time
- Access to the SharePoint site and **Real Estate Properties** Microsoft List containing the sample properties

---

## Lab Architecture
By the end of this lab, you will have:
- A **Get Property** agent flow that finds a property in the **Real Estate Properties** list
- A **Real Estate Booking Requests** list for submitted booking requests
- A **Create Booking Request** agent flow that adds a booking to the requests list
- Both flows added to the agent and called from the showing topic
- Tested and verified property lookup and booking request creation

> **Note:** The property list stores property records. Booking requests are saved separately so they do not become property records.

---

## Step 1: Create the Booking Requests List

In this step, you will create a Microsoft List to store the requests submitted by the agent.

### 1.1 Open Microsoft Lists
1. Open a new browser tab and sign in to [Microsoft Lists](https://www.microsoft.com/en-us/microsoft-365/microsoft-lists).

### 1.2 Create a Blank List
1. Select **+ Create new**.
2. Select **Blank list**.
3. Configure the list:
   - **Name:** `Real Estate Booking Requests`
4. Select **Create**.

### 1.3 Add Columns
1. Select the **Title** column header > **Column settings** > **Rename**, and rename it to `Booking Name`. Select **Save**.
2. Select **+ Add column** and add the following columns, one at a time. For each one, select the column type, select **Next**, enter the name, and select **Save**:

| Column name | Column type |
|---|---|
| `Property ID` | Number |
| `Property Name` | Text |
| `Viewer Name` | Text |
| `Viewer Email` | Text |
| `Requested Date and Time` | Text |

3. Copy the URL of the list and keep the browser tab open. You will select its SharePoint site when configuring the agent flow.

---

## Step 2: Create the Get Property Agent Flow

In this step, you will create an agent flow that searches the **Real Estate Properties** list for a property with the requested number of bedrooms.

### 2.1 Create the Flow and Add an Input
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment.
2. Select **Agents** in the left navigation pane, then open the **Real Estate Booking Service** agent.
3. Select the **Tools** tab > **+ Add a tool** > **+ New tool** > **Agent flow**.
4. In the flow designer, select the **When an agent calls the flow** trigger, then select **+ Add an input**.
5. Select **Text**.
6. Enter `Bedrooms` for **Input** and `Number of Bedrooms` for **Please enter your input**.
7. Select **Save draft**.
8. Select the **Overview** tab. In the **Details** section, select **Edit**.
9. Rename the flow `Get Property`, then select **Save**.
10. Return to the **Designer** tab.

### 2.2 Get a Matching Property from Microsoft Lists
1. Select the **+** icon between the trigger and **Respond to Copilot**.
2. Search for **SharePoint**, then select the SharePoint connector's **Get items** action.
3. If prompted, sign in with the account that can access the SharePoint site containing the lists.
4. Configure **Get items**:
   - **Site Address:** Select the site containing **Real Estate Properties**.
   - **List Name:** Select **Real Estate Properties** (or the unique list name you created).
   - **Filter Query:** Enter `Bedrooms eq `, including the space after `eq`, then insert the **Bedrooms** trigger input using dynamic content.
   - **Top Count:** Enter `1`.

> **Note:** The topic's bedroom answer is passed to the flow as text, but `Bedrooms` is a number column. Do not enclose the filter value in quotation marks; SharePoint expects a numeric value in the filter query. In SharePoint, renaming the default **Title** column to **Property Name** does not change its internal name; the flow uses `Title` to retrieve that value.

### 2.3 Return the Property to the Agent
1. Select **Respond to Copilot** and select **+ Add an output**.
2. Select **Number**.
3. Enter `propertyid` for **Enter a name**.
4. Select the value field and select **fx** (Insert Expression).
5. Enter:
   ```text
   first(body('Get_items')?['value'])?['ID']
   ```
6. Select **Add**.
7. Select **+ Add an output**, then select **Text**.
8. Enter `propertyname` for **Enter a name**.
9. Select the value field and select **fx** (Insert Expression).
10. Enter:
    ```text
    first(body('Get_items')?['value'])?['Title']
    ```
11. Select **Add**.
12. Open the **Settings** tab for **Respond to Copilot**. Ensure **Asynchronous Response** is **Off**, then select **Done**.
13. Select **Save draft**, wait for the save to complete, then select **Publish**.
14. Return to the **Real Estate Booking Service** agent and open the **Tools** tab.
15. If **Get Property** is not listed, select **+ Add a tool** > **Flow**, select **Get Property**, then select **Add and configure**.
16. Open **Get Property**, set its description to `Get properties with the right number of bedrooms.`, then select **Save**.
17. Confirm that **Get Property** appears in the agent's tools.

---

## Step 3: Add the Get Property Tool to the Showing Topic

In this step, you will replace the manual property question with the **Get Property** tool.

1. Select the **Topics** tab and open **Book a Real Estate Showing**.
2. Select the **+** icon below the **How many bedrooms do you need?** question, select **Add a tool**, then select **Get Property**.
3. For the **Bedrooms** input, select the **NumberofBedrooms** variable.
4. Select the ellipsis **…** on the **Which property do you want to see?** question node and select **Delete**.
5. Select the **+** icon below the **Get Property** action and select **Send a message**.
6. Enter `Property ` in the message field, including a space after the word.
7. Select the **{X}** (Insert variable) icon and insert the `propertyname` output from **Get Property**.
8. Select **Save**.

---

## Step 4: Create the Create Booking Request Agent Flow

In this step, you will create an agent flow that saves the selected property and customer details to the **Real Estate Booking Requests** list.

### 4.1 Create the Flow and Add Inputs
1. In the agent, select the **Tools** tab > **+ Add a tool** > **+ New tool** > **Agent flow**.
2. Select **Save draft** and wait for the flow to save.
3. Select the **Overview** tab. In the **Details** section, select **Edit**.
4. Rename the flow `Create Booking Request`, then select **Save**.
5. Return to the **Designer** tab.
6. Select the **When an agent calls the flow** trigger and add these inputs:

   | Type | Input | Please enter your input |
   |---|---|---|
   | Number | `PropertyId` | `Property ID` |
   | Text | `PropertyName` | `Property Name` |
   | Text | `ViewerName` | `Viewer Name` |
   | Text | `ViewerEmail` | `Viewer Email` |
   | Text | `RequestedDateTime` | `Requested Date and Time` |

### 4.2 Create a List Item
1. Select the **+** icon between the trigger and **Respond to Copilot**.
2. Search for **SharePoint**, then select the SharePoint connector's **Create item** action.
3. If prompted, sign in with the account that can access the SharePoint site.
4. Configure **Create item**:
   - **Site Address:** Select the site containing **Real Estate Booking Requests**.
   - **List Name:** Select **Real Estate Booking Requests**.
   - **Booking Name:** Enter `Copilot booking`.
   - **Property ID:** Insert the **PropertyId** trigger input.
   - **Property Name:** Insert the **PropertyName** trigger input.
   - **Viewer Name:** Insert the **ViewerName** trigger input.
   - **Viewer Email:** Insert the **ViewerEmail** trigger input.
   - **Requested Date and Time:** Insert the **RequestedDateTime** trigger input.
5. Select **Respond to Copilot** and open its **Settings** tab.
6. Ensure **Asynchronous Response** is **Off**.
7. Select **Save draft**, wait for the save to complete, then select **Publish**.
8. Return to the **Real Estate Booking Service** agent and select the **Tools** tab.
9. If **Create Booking Request** is not listed, select **+ Add a tool** > **Flow**, select **Create Booking Request**, then select **Add and configure**.
10. Set its description to `Save a real estate showing request to the booking requests list.`, then select **Save**.
11. Confirm that **Create Booking Request** appears in the agent's tools.

---

## Step 5: Add the Create Booking Request Tool to the Topic

1. Select the **Topics** tab and open **Book a Real Estate Showing**.
2. Below the message that displays the property name, select **+** > **Add a tool**, then select **Create Booking Request**.
3. Map the inputs to the topic variables:
   - **PropertyId:** `propertyid` output from **Get Property**
   - **PropertyName:** `propertyname` output from **Get Property**
   - **ViewerName:** `Name`
   - **ViewerEmail:** `EmailAddress`
   - **RequestedDateTime:** The variable that stores the response to the date-and-time question
4. Select the **Create Booking Request** action. Use Copilot to generate a confirmation message that thanks the user and confirms the showing request was saved.
5. Review and update the generated message as needed.
6. Add **Topic management** > **End all topics** after the confirmation message.
7. Select **Save**, then select **Publish** and confirm publishing the agent.

---

## Step 6: Test the Agent and Verify the Request

### 6.1 Test the End-to-End Conversation
1. Open the **Test your agent** pane. If needed, enable **Track between topics** from the ellipsis **…** menu.
2. Start a new test session.
3. Enter the trigger phrase:
   ```text
   I want to book a real estate showing
   ```
4. Enter a fictitious name and email address, then select **Yes** to confirm the details.
5. Select a property type.
6. Enter `3` for the number of bedrooms.
7. Verify that the agent displays **1100 High Villas**.
8. Enter a requested showing date and time when prompted.
9. Verify that the agent displays a confirmation message.

### 6.2 Verify the Booking Request
1. Open the **Real Estate Booking Requests** list in SharePoint.
2. Verify that a new item was added with the property, customer, and requested date-and-time details from the test conversation.

---

## Verification Checklist
- [ ] **Real Estate Booking Requests** list created with the required columns
- [ ] **Get Property** flow filters the **Real Estate Properties** list by bedroom count
- [ ] **Get Property** returns a property ID and property name
- [ ] The showing topic calls **Get Property** and displays the returned property name
- [ ] **Create Booking Request** flow creates an item in the booking requests list
- [ ] The showing topic passes the selected property and customer details to **Create Booking Request**
- [ ] A test for a 3-bedroom property returns **1100 High Villas**
- [ ] The test conversation completes and its booking request is visible in the list

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Created a Microsoft List for booking requests
2. ✅ Created an agent flow that retrieves a property from a Microsoft List
3. ✅ Used a SharePoint **Get items** action with a filter query
4. ✅ Returned list item data to the agent
5. ✅ Created an agent flow that saves a booking request to a Microsoft List
6. ✅ Called both flows from a topic and tested the end-to-end conversation

### Key Learnings
- **Agent flows:** Let an agent call Power Automate flows to work with connected services
- **SharePoint connector:** Provides actions such as **Get items** and **Create item** for Microsoft Lists
- **Filter queries:** Match list items to user-provided values; numeric values are not quoted
- **Flow inputs and outputs:** Pass topic variables into a flow and return selected values to the agent
- **List design:** Separate lists can keep different types of records, such as properties and booking requests, organized
- **Synchronous responses:** With asynchronous response off, the agent waits for the flow to finish before continuing
- **Testing:** An end-to-end conversation verifies both the agent's response and the item saved in the list

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Create Agent Flows with Microsoft Lists** lab.

You now have hands-on experience with:
- Connecting agent flows to Microsoft Lists
- Filtering list items and returning values to an agent
- Creating list items from a topic conversation
- Testing and verifying an agent flow end to end

---

**End of Lab**
