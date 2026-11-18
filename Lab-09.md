# Lab: Call External APIs in Copilot Studio

## Overview
In this hands-on lab, you will build a topic in the **Real Estate Booking Service** agent that calls an external API and uses a prompt to answer questions about the returned data. You will examine the request and response, collect a city from the user, and configure a conversation that supports follow-up questions.

**Prerequisites:**
- A **Real Estate Booking Service** agent in a Copilot Studio environment
- Permission to create topics, send HTTP requests, and create and run prompts in that environment
- Access to a RapidAPI account and the **private-zillow** API

> **Important:** Review the API provider's terms, plan limits, and any charges before subscribing. Only use API data in the prompt if your organization's policies and the provider's terms permit it.

---

## Lab Architecture
By the end of this lab, you will have:
- Subscribed to an external API and inspected its request and JSON response
- Created a topic that collects a city and sends an HTTP request
- Stored the API response in a topic variable and converted it to JSON
- Created a prompt that answers questions using the returned data
- Tested a follow-up loop that reuses the response

---

## Step 1: Subscribe to and Analyze an External API

In this step, you will subscribe to the API and examine a sample request and response.

### 1.1 Subscribe to the API
1. Navigate to [RapidAPI](https://rapidapi.com/) and sign up or sign in
2. Open the **API Hub** and search for `private-zillow`
3. Select **Subscribe to Test** in the upper-right corner
4. Review the available plans, select **Start Free Plan**, and select **Subscribe**

> **Note:** API availability, plan names, quotas, and pricing can change. If the API or plan is unavailable, check with your instructor before continuing.

### 1.2 Examine the Request
1. In the **Find an Agent** section, select the **GET /agent/search** endpoint
2. Select the **Params** tab and review the available parameters
3. Under **Query Params**, set `location` to `Redmond`, leave `page` as `1`, and remove the `agentName` value
4. Review the **Code Snippets** tab and identify:
   - The **GET** method
   - The URL and query parameters, including `location=Redmond` and `page=1`
   - The `x-rapidapi-host` and `x-rapidapi-key` headers
   - The absence of a request body
5. Keep the host value and API key available for the HTTP request configuration

> **Important:** Treat the API key as a secret. Do not share it in chat, screenshots, source files, or sample JSON. The HTTP node in this lab stores the key in the topic configuration, so restrict author access and use a dedicated lab key. For production, use an approved connector or credential-management approach rather than embedding a key in a topic.

### 1.3 Test the Endpoint
1. Select **Test Endpoint**
2. Inspect the JSON response and the returned agent information
3. Copy the full successful JSON response for use as sample data later in the lab

> **Note:** Results and the number of agents may vary. Do not proceed with an error response as sample JSON; check the subscription, request parameters, and quota first.

---

## Step 2: Create the Search Topic

In this step, you will create a topic that asks the user for a city.

### 2.1 Create a Topic with Copilot
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment
2. Select **Agents** and open the **Real Estate Booking Service** agent
3. Select the **Topics** tab
4. Select **+ Add a topic** > **Add from description with Copilot**
5. Configure the topic:
   - **Name your topic:** `Search for a Real Estate Agent`
   - **Create a topic to…:** `Searches for a Real Estate Agent in a given city.`
6. Select **Create**
7. Verify that the topic has a trigger and a question that asks for a city. Remove any unnecessary generated nodes.
8. In the city question, select the variable under **Save user response as** and name it `City`
9. Select **Save**

---

## Step 3: Call the External API

In this step, you will build a URL from the user's city, send the request, and save the response.

### 3.1 Construct the Request URL
1. Under the city question, select the **+** icon and select **Variable management** > **Set a variable value**
2. Under **Select a variable**, select **Create a new variable** and name it `URL`
3. In **To value**, select the ellipsis **…** and select **Formula**
4. Enter the following Power Fx formula:
   ```powerfx
   Concatenate(
       "https://private-zillow.p.rapidapi.com/agent/search?location=",
       EncodeUrl(Topic.City),
       "&page=1"
   )
   ```
5. Select **Insert**

> **Note:** The formula combines the API URL with the city supplied by the user. `EncodeUrl` encodes spaces and other special characters in the city value.

### 3.2 Configure the HTTP Request
1. Under the set-variable node, select **+** > **Advanced** > **Send HTTP request**
2. Set the request **Method** to **GET**
3. In the **URL** field, select the ellipsis **…** and select the `URL` variable
4. Under **Headers and body**, select **Edit**
5. Under **Headers**, select **+ Add** and enter:
   - **Key:** `x-rapidapi-host`
   - **Value:** The host value from the RapidAPI code snippet
6. Select **+ Add** again and enter:
   - **Key:** `x-rapidapi-key`
   - **Value:** Your RapidAPI key
7. Select **Save**
8. Under **Response data type**, select **From sample data**
9. Select **</> Get schema from sample JSON**
10. Paste the successful JSON response copied from the endpoint test and select **Confirm**
11. Under **Save response as**, select **Create a new variable** and name it `AgentData`
12. Select **Save**

### 3.3 Convert the Response to JSON
1. Under the HTTP request node, select **+** > **Variable management** > **Set a variable value**
2. Create a new variable named `AgentDataJSON`
3. In **To value**, select the ellipsis **…**, open the **Formula** tab, and enter:
   ```powerfx
   JSON(Topic.AgentData)
   ```
4. Select **Insert**, then select **Save**

> **Note:** The HTTP request stores the parsed response in `AgentData`. The formula converts it to JSON text for the prompt.

---

## Step 4: Add a Prompt and Follow-Up Questions

In this step, you will combine the user's question and API data in a prompt, display the answer, and let the user ask another question.

### 4.1 Ask What the User Wants to Know
1. Under the `AgentDataJSON` set-variable node, select **+** > **Ask a question**
2. Configure the question:
   - **Question:** `What would you like to know?`
   - **Identify:** **User's entire response**
   - **Save user response as:** Create a new variable named `UserQuery`

### 4.2 Create and Test the Prompt
1. Under the question node, select **+** > **Add a tool** > **New prompt**
2. Set the prompt title to `Retrieve Real Estate Agent Information`
3. In **Instructions**, enter:
   ```
   You will provide an answer based on the user query and JSON data provided.
   ```
4. Set the model to **GPT Latest**, or select another approved model available in your environment
5. In the instructions, select **Add content** > **Text** and name the input `Prompt`
6. In the input's **Sample data**, enter:
   ```
   Provide the top 5 agents by sales
   ```
7. Add a new line and paste the successful JSON response from the endpoint test
8. Select **Close**
9. Select **Test** and review the response
10. Select **Save**

### 4.3 Connect the Prompt Inputs and Output
1. In the prompt node, under **Inputs**, select the ellipsis **…** for **Prompt (String)** and select **Formula**
2. Enter:
   ```powerfx
   Concatenate(
       Topic.UserQuery,
       " ",
       Topic.AgentDataJSON
   )
   ```
3. Under **Outputs**, select the variable field for `predictionOutput`, create a new variable, and name it `Response`
4. Immediately below the prompt node, add a **Send a message** node, or use an existing message node at that position
5. Remove any existing text from the message, select **Insert variable** `{x}`, and select `Response.text`

> **Note:** The prompt receives the current question and the API response together. Review generated answers against the response; the model can make mistakes or encounter missing data.

### 4.4 Add a Follow-Up Loop
1. Under the message node, select **+** > **Ask a question**
2. Configure the question:
   - **Question:** `Would you like to ask more?`
   - **Identify:** **Multiple choice options**
   - **Options for user:** `Yes` and `No`
   - **Save user response as:** `UserChoice`
3. Check that condition branches appear below the question. If they do not, select **+** > **Add a condition**.
4. Set the first condition to `UserChoice` **is equal to** `No`
5. If there is a separate condition between the first condition and **All other conditions**, delete it
6. Under **All other conditions**, select **+** > **Topic management** > **Go to step**
7. Select the earlier **What would you like to know?** question
8. Select **Save**

> **Note:** The loop reuses the JSON already retrieved for the original city; it does not make another API request.

---

## Step 5: Test the Topic

### 5.1 Test the Initial Question and Follow-Up
1. Select the agent's **Overview** tab and open the **Test your agent** pane
2. Enable **Track between topics** and start a new test session
3. Enter:
   ```
   I am looking for a new real estate agent.
   ```
4. When prompted for a city, enter `Redmond`
5. When asked what you would like to know, enter:
   ```
   I want to know the top 5 agents based on sales.
   ```
6. Review the response against the returned API data. Results may vary, and the response may contain fewer than five agents or lack the requested sales information.
7. Select **Yes** when asked whether you would like to ask more
8. Ask for more details about an agent present in the returned response, for example:
   ```
   Give me more details about the first agent.
   ```
9. Review the answer, then select **No** to finish

> **Note:** If the request fails, check the API subscription, quota, host, and key without exposing the key. If the response lacks the requested information, the agent should not be expected to invent it.

---

## Verification Checklist
- [ ] API subscription and successful sample response obtained
- [ ] Request method, URL, query parameters, and headers reviewed
- [ ] **Search for a Real Estate Agent** topic collects a city
- [ ] HTTP request uses the encoded city and saves its response as `AgentData`
- [ ] Response converted to `AgentDataJSON`
- [ ] Prompt receives the user question and JSON and displays `Response.text`
- [ ] Follow-up loop returns to the question when the user selects **Yes**
- [ ] Initial question and follow-up tested in the Test pane

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Subscribed to an external API and examined a request and response
2. ✅ Created a topic that collects a city and calls the API
3. ✅ Stored and converted the response for use in a prompt
4. ✅ Created a prompt to answer questions about the data
5. ✅ Added and tested a follow-up loop

### Key Learnings
- **API requests:** Methods, URLs, query parameters, and headers determine how an external API is called
- **User input:** Topic variables and Power Fx formulas can construct a request from a user's city
- **URL encoding:** Encoding the city helps handle spaces and special characters in query parameters
- **Credentials:** API keys require restricted access and should not be shared
- **JSON:** A structured API response can be stored in a variable and converted to text for a prompt
- **Prompt actions:** Combine a user's question with returned data to generate a conversational answer
- **Follow-up loops:** Reuse the response for additional questions without making another request
- **Testing:** Check API responses and generated answers, including missing data and request failures

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Call External APIs in Copilot Studio** lab.

You now have hands-on experience with:
- Inspecting and testing an external API
- Sending HTTP requests from a topic
- Working with response variables and JSON
- Creating prompt actions and follow-up loops
- Testing an API-backed conversation

---

**End of Lab**
