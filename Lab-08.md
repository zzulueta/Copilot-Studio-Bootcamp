# Lab: Create Property Listings from Files with Prompts

## Overview
In this hands-on lab, you will build a **Real Estate Booking Service** topic that accepts a property listing file, extracts structured details with a prompt, and creates a new item in the **Real Estate Properties** Microsoft List using an agent flow.

**Prerequisites:**
- A **Real Estate Booking Service** agent in your Copilot Studio environment
- Access to the SharePoint site containing the **Real Estate Properties** Microsoft List
- A sample property listing file, such as `Listing 1.pdf`

---

## Lab Architecture
By the end of this lab, you will have:
- A **Create Property Listing** topic that accepts a file
- A prompt action that extracts property details in JSON format
- A **Create Property Listing** agent flow that adds an item to the **Real Estate Properties** list
- A confirmation message and a verified list item

> **Note:** The flow adds property listings to the existing list. It does not create a new list or add booking requests.

---

## Step 1: Create the Property Listing Topic

In this step, you will create a topic that asks the user to upload a property listing.

1. Open the **Real Estate Booking Service** agent and select the **Topics** tab.
2. Select **+ Add a topic** > **Add from description with Copilot**.
3. Enter:
   - **Name your topic:** `Create Property Listing`
   - **Create a topic to:** `Create a property listing in the Real Estate Properties Microsoft List from an uploaded file`
4. Select **Create**.
5. Delete the generated actions, keeping only the **Trigger** node.
6. Add a **Question** action after the trigger and configure it:
   - **Message:** `Please upload your property file.`
   - **Identify:** **File**
   - **Save user response as:** `fileUpload`
7. Select **Save**.

---

## Step 2: Create a Prompt to Extract Property Details

In this step, you will create a prompt that reads the uploaded file and returns the property information in a consistent structure.

1. Under the file question, select **+** > **Add a tool** > **New prompt**.
2. Configure the prompt:
   - **Name:** `Get Property Details from File`
   - **Model:** `GPT Latest`or select another approved model available in your environment
   - **Output:** `JSON`
3. In **Instructions**, enter:
   ```text
   Extract the property information from the supplied file. Return a JSON object with these fields:
   - PropertyName (text)
   - AskingPrice (number only, without currency symbols)
   - Street (text)
   - City (text)
   - Bedrooms (number)
   - Bathrooms (number)

   Use only information present in the file. Do not guess or invent missing values.
   ```
4. Select **Add content** > **Image or document**, upload `Listing 1.pdf` found from the Lab-08 folder, then close the content pane.
5. Select **Test** and verify that the response contains the property fields in JSON format.
6. Select **Save**.
7. In the prompt node, select the ellipsis **…** and add the `fileUpload` variable as the prompt input.
8. Under **Outputs**, select **Create a new variable** and name it `predictionOutput`.
9. Select **Save**.

---

## Step 3: Create the Create Property Listing Agent Flow

In this step, you will create an agent flow that adds the extracted property information to the existing Microsoft List.

### 3.1 Create the Flow and Add Inputs
1. Open the **Real Estate Booking Service** agent and select the **Tools** tab.
2. Select **+ Add a tool** > **Agent flow**.
3. In the flow designer, select the **When an agent calls the flow** trigger and add these inputs:

   | Type | Input | Please enter your input |
   |---|---| --- |
   | Text | `PropertyName` | The name of the property |
   | Text | `Street` | The street address of the property |
   | Text | `City` | The city where the property is located | 
   | Number | `AskingPrice` | The asking price of the property |
   | Number | `Bathrooms` | The number of bathrooms in the property |
   | Number | `Bedrooms` | The number of bedrooms in the property |

4. Select **Save draft**.

### 3.2 Add the Property to the Microsoft List
1. Select the **+** icon after the trigger.
2. Search for **SharePoint** and select the SharePoint connector's **Create item** action.
3. If prompted, sign in with an account that can add items to the **Real Estate Properties** list.
4. Configure **Create item**:
   - **Site Address:** select **Enter custom value** and enter the site URL that contains **Real Estate Properties**. 
   - **List Name:** Select **Real Estate Properties**.
   - **Title**: Insert the **PropertyName** trigger input.
   - **Street:** Insert the **Street** trigger input.
   - **City:** Insert the **City** trigger input.
   - **Asking Price:** Insert the **AskingPrice** trigger input.
   - **Bathrooms:** Insert the **Bathrooms** trigger input.
   - **Bedrooms:** Insert the **Bedrooms** trigger input.
   - **Owner:** Leave blank if the field is optional. The uploaded file does not provide an owner value.
5. Select **Save draft**.
6. Open the **Overview** tab. In the **Details** section, select **Edit** and set:
   - **Name:** `Create Property Listing`
   - **Description:** `Create a property listing in the Real Estate Properties Microsoft List.`
7. Select **Save**, return to the **Designer** tab, then select **Publish**.

---

## Step 4: Add the Agent Flow to the Topic

In this step, you will connect the prompt's extracted values to the agent flow and confirm the result to the user.

1. Select the **Topics** tab and open **Create Property Listing**.
2. Under the **Get Property Details from File** prompt node, select **+** > **Add a tool** and select **Create Property Listing**.
3. Map each flow input to the matching field in `predictionOutput`:

   | Agent flow input | Prompt output |
   |---|---|
   | `PropertyName` | `PropertyName` |
   | `Street` | `Street` |
   | `City` | `City` |
   | `AskingPrice` | `AskingPrice` |
   | `Bathrooms` | `Bathrooms` |
   | `Bedrooms` | `Bedrooms` |

4. Select **Save**.
5. Add a **Send a message** action after the agent flow.
6. Enter `The property has been saved: `, then insert the `PropertyName` value from `predictionOutput`.
7. Add **Topic management** > **End current topic** after the message.
8. Select **Save**.

---

## Step 5: Test the Topic and Verify the List Item

### 5.1 Test the Topic
1. Open the **Test your agent** pane and enable **Track between topics** if it is not already enabled.
2. Enter:
   ```text
   Create a property
   ```
3. Upload `Listing 1.pdf` using the paperclip icon and submit it.
4. Verify that the agent displays a confirmation containing the property name.

### 5.2 Verify the New Property
1. Open **Microsoft Lists** and navigate to **Real Estate Properties**.
2. Verify that a new item was added with the property name, asking price, street, city, bedrooms, and bathrooms extracted from the file.

---

## Verification Checklist
- [ ] **Create Property Listing** topic asks the user to upload a file
- [ ] The prompt extracts the six property fields in JSON format
- [ ] The **Create Property Listing** flow uses the SharePoint **Create item** action
- [ ] The flow adds the extracted property data to **Real Estate Properties**
- [ ] The topic maps each prompt output to the matching flow input
- [ ] The agent confirms the property name after creating the item
- [ ] The new property can be verified in Microsoft Lists

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Created a topic that accepts a property listing file
2. ✅ Used a prompt to extract structured property details
3. ✅ Created an agent flow that adds an item to a Microsoft List
4. ✅ Mapped prompt outputs to flow inputs
5. ✅ Tested the workflow and verified the new list item

### Key Learnings
- **File questions:** Let users provide documents as inputs to a topic
- **Prompt actions:** Extract structured values from unstructured documents
- **JSON outputs:** Provide named fields that can be mapped to downstream actions
- **Agent flows:** Connect topic variables to actions in Power Automate
- **Microsoft Lists:** Store structured property records for the agent to use
- **Field mapping:** Match each extracted value to its corresponding list column
- **End-to-end testing:** Verifies both the conversational confirmation and the saved record

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Create Property Listings from Files** lab.

You now have hands-on experience with:
- Collecting a file in a topic
- Extracting property information with a prompt
- Creating Microsoft List items with an agent flow
- Testing a file-to-list workflow end to end

---

**End of Lab**
