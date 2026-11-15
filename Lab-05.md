# Lab: Work with Entities in Copilot Studio

## Overview
In this hands-on lab, you will explore prebuilt entities and create custom entities for the **Real Estate Booking Service** agent. You will use those entities in the **Book a Real Estate Showing** topic to collect a property type and a valid number of bedrooms, then test the conversation.

**Prerequisites:**
- A **Real Estate Booking Service** agent in your Copilot Studio environment, configured to use classic orchestration
- A **Customer Details** topic that collects and confirms customer information
- A **Book a Real Estate Showing** topic that calls **Customer Details** and has a **Yes** condition branch followed by property and date-and-time questions

---

## Lab Architecture
By the end of this lab, you will have:
- Reviewed the agent's prebuilt entities
- Created a **Property Type** closed list entity with synonyms and smart matching
- Created a **Number of Bedrooms** regular expression entity
- Added questions that use the custom entities to the showing topic
- Tested selectable property types and the custom retry prompt

> **Note:** This lab collects information in the conversation. It does not connect to a Microsoft List, save a booking request, or schedule a showing.

---

## Step 1: Create Custom Entities

Entities help an agent identify structured information in a user's response. Copilot Studio includes prebuilt entities for common types of information, and you can create custom entities for your own scenarios.

### 1.1 View Prebuilt Entities
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment
2. Select **Agents** in the left navigation pane
3. Select the **Real Estate Booking Service** agent
4. Select **Settings** in the upper-right corner
5. Select the **Entities** tab and review the list of prebuilt entities

### 1.2 Create the Property Type Entity
1. Select **+ Add an entity** > **+ New entity**
2. Select **Closed list**
3. In the **Name** field, enter `Property Type`
4. Enter each item below in **Enter item**, selecting **Add** after each one:
   - `Apartment`
   - `Condominium`
   - `Duplex`
   - `House`
5. Add synonyms for the following items. For each item, select **+ Synonyms**, enter the synonym, select the **+** icon, and select **Done**:

   | Item | Synonym |
   |---|---|
   | Apartment | `Flat` |
   | Condominium | `Townhouse` |
   | House | `Single-family home` |

6. Enable **Smart matching**
7. Select **Save**, then close the **Property Type** window

> **Note:** The synonym mappings are examples for this exercise. A townhouse is not necessarily a condominium in real-world property classifications.

### 1.3 Create the Number of Bedrooms Entity
1. Select **+ Add an entity** > **+ New entity**
2. Select **Regular expression (Regex)**
3. In the **Name** field, enter `Number of Bedrooms`
4. In the **Pattern** field, enter:
   ```regex
   ^[1-5]$
   ```
5. Select **Save**, then close the **Number of Bedrooms** pane
6. Select the **X** icon in the upper-right corner to close Settings and return to your agent

> **Note:** The pattern matches a single digit from 1 through 5.

---

## Step 2: Use Entities in the Showing Topic

In this step, you will add questions that use the custom entities to capture property preferences.

### 2.1 Add a Property Type Question
1. Select the **Topics** tab and open **Book a Real Estate Showing**
2. In the **Yes** condition branch, select the **+** icon between the condition and the existing property question, then select **Ask a question**
3. Configure the question:
   - **Enter a message:** `What type of property do you want to see?`
   - **Identify:** **Property Type**
   - **Select options for user:** Check **Display** for all four values
   - **Save user response as:** Select the variable and set its name to `PropertyType`

### 2.2 Add a Number of Bedrooms Question
1. Select the **+** icon below the new property type question and select **Ask a question**
2. Configure the question:
   - **Enter a message:** `How many bedrooms do you need?`
   - **Identify:** **Number of Bedrooms**
   - **Save user response as:** Select the variable and set its name to `NumberofBedrooms`
3. Select **Save**
4. On the **NumberofBedrooms** question node, select the ellipsis **…** to open its properties
5. Select **Question behavior**
6. Under **Retry prompt**, enable **Customize**
7. Enter the retry prompt:
   ```
   Enter a value from 1 to 5.
   ```
8. Select **Save**

---

## Step 3: Test the Entity Questions

### 3.1 Test Property Types and Bedroom Validation
1. Open the **Test your agent** pane and start a new test session
2. Enter the trigger phrase:
   ```
   I want to book a real estate showing
   ```
3. Enter fictitious sample customer details and select **Yes** when asked to confirm them
4. Verify that all four property types are displayed, then select one
5. When asked for the number of bedrooms, enter:
   ```
   6
   ```
6. Verify that the agent displays the retry prompt **Enter a value from 1 to 5.**
7. Enter `3`, then complete the existing property and date-and-time questions
8. Verify that the conversation continues to the final message

---

## Verification Checklist
- [ ] Prebuilt entities reviewed
- [ ] **Property Type** closed list created with four items, synonyms, and smart matching
- [ ] **Number of Bedrooms** regex entity created with the pattern `^[1-5]$`
- [ ] Property type and bedroom questions added to the **Yes** branch before the existing property question
- [ ] Responses saved as `PropertyType` and `NumberofBedrooms`
- [ ] All four property types displayed in the test conversation
- [ ] An invalid bedroom value triggers the custom retry prompt, and a valid value allows the conversation to continue

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Reviewed prebuilt entities
2. ✅ Created a closed list entity with synonyms and smart matching
3. ✅ Created a regular expression entity for bedroom counts
4. ✅ Used custom entities in topic question nodes
5. ✅ Tested selectable options and a retry prompt

### Key Learnings
- **Entities:** Help the agent recognize structured information in user responses
- **Prebuilt entities:** Provide ready-to-use recognition for common types of information
- **Custom entities:** Tailor recognition to a specific business scenario
- **Closed lists:** Define a limited set of values and can include synonyms and smart matching
- **Regular expressions:** Describe patterns for inputs such as a single digit from 1 to 5
- **Question nodes:** Use entities to identify responses and store them in variables
- **Retry prompts:** Guide users when an answer does not match the expected entity
- **Testing:** Valid and invalid inputs help verify recognition and conversation flow

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Work with Entities in Copilot Studio** lab.

You now have hands-on experience with:
- Reviewing prebuilt entities
- Creating closed list and regular expression entities
- Adding synonyms and enabling smart matching
- Using entities in question nodes
- Configuring retry prompts and testing user input

---

**End of Lab**
