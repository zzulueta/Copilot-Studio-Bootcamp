# Lab: Use MCP Servers in Copilot Studio

## Overview
In this hands-on lab, you will extend the **Real Estate Booking Service** agent with Model Context Protocol (MCP) servers. You will configure the **Mail MCP** server to search and read sample email, then add the **Teams MCP** server to post a property update to a Teams channel.

**Prerequisites:**
- A **Real Estate Booking Service** agent in a Copilot Studio environment
- A **Create Property Listing** topic and agent flow that create an item in the **Real Estate Properties** Microsoft List from an uploaded property file
- A sample property listing file and permission to add items to the **Real Estate Properties** list
- A Microsoft 365 account with an accessible mailbox and permission to create or use a Teams team and channel
- Access to the **Mail** and **Teams** MCP servers and permission to create their connections
- Permission to configure agent authentication

> **Important:** Microsoft documents the application-specific Mail and Teams MCP servers as preview features intended for backward compatibility, not production or new projects. Availability, tool names, and permissions can change. For production guidance, see the [Mail reference](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-mail-tools) and [Teams reference](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-teams-tools).

---

## Lab Architecture
By the end of this lab, you will have:
- A **Mail MCP** connection configured with only the tools needed to search and read messages
- A sample email that the agent can find and summarize
- A **Teams MCP** connection configured to find teams and channels and post a message
- A property listing saved in Microsoft Lists and a sample update posted to a Teams channel

---

## Step 1: Configure the Agent for MCP

MCP servers expose tools that an agent can use to work with connected services. Generative orchestration selects appropriate tools based on the user's request.

### 1.1 Open the Agent
1. Navigate to [Copilot Studio](https://copilotstudio.microsoft.com/) and make sure you are in the appropriate environment
2. Select **Agents** in the left navigation pane
3. Open the **Real Estate Booking Service** agent

### 1.2 Enable Generative Orchestration
1. Select **Settings** in the upper-right corner
2. On the **Generative AI** page, go to **Orchestration**
3. Enable **Generative orchestration** and select **Save**
4. Close **Settings**

> **Note:** Generative orchestration is required for MCP tools. Changing from classic orchestration can affect how existing topics are selected; test your existing workflows after making the change.

### 1.3 Require Authentication
1. Select **Settings**, then select **Security** > **Authentication**
2. Select **Authenticate with Microsoft** and select **Save**
3. If the agent is already published, publish the authentication change before adding the MCP tools
4. Close **Settings**

> **Important:** Authentication changes affect a published agent only after publishing. Publishing also releases other saved changes. Do not expose an agent with mailbox or Teams access through an anonymous channel. If your organization requires a different authentication configuration, follow its policy; some channels require manual authentication.

---

## Step 2: Add and Test the Mail MCP Server

In this step, you will allow the agent to search and read a sample message without enabling mail-writing tools.

### 2.1 Create a Sample Email
1. Open Outlook using the account you will connect to the Mail MCP server
2. Send an email to your own mailbox:
   - **Subject:** `Property update - Copilot Studio test`
   - **Body:** `A sample property at 1100 High Villas in Redmond is available for viewing.`
3. Verify that the message appears in your mailbox

> **Note:** Use only fictitious information for this exercise. Mail tools operate on the mailbox accessible through the configured connection.

### 2.2 Add the Mail MCP Server
1. Return to the agent in Copilot Studio and select the **Tools** tab
2. Select **+ Add a tool**, then select **Model Context Protocol**
3. Search for `Mail` and select the **Mail** MCP server
4. Verify the connection. If needed, select **Create new connection** and follow the sign-in and consent prompts.
5. Select **Add and configure**
6. Review the available tools and turn off **Allow all**
7. Enable only the tools for searching and retrieving messages:
   - `mcp_MailTools_graph_mail_searchMessages`
   - `mcp_MailTools_graph_mail_getMessage`
8. Leave sending, drafting, updating, and deleting tools disabled, then select **Save**

> **Note:** Tool names may differ in your environment. Use the tools with the corresponding search and get-message functions, and consult the server's documentation if the names have changed. If Mail MCP is unavailable or your organization requires administrator consent, ask your administrator before continuing.

### 2.3 Test the Mail Connection
1. Open the **Test your agent** pane, enable **Track between topics**, and start a new test session
2. Enter:
   ```text
   Find the email with the subject "Property update - Copilot Studio test" and summarize it.
   ```
3. If prompted to connect or authorize the Mail MCP server, follow the prompts and select **Allow**
4. Verify that the response reflects the sample message; compare it with the email in Outlook
5. Review the test activity to verify that the Mail MCP search or read tool was used

> **Note:** Disabling the Mail MCP write tools limits this server's actions; it does not restrict other enabled tools or the underlying mailbox permissions.

---

## Step 3: Add the Teams MCP Server

In this step, you will create a channel for property updates and configure the tools needed to post to it.

### 3.1 Create a Team and Channel
1. Open Microsoft Teams and select **Chat** in the left navigation pane
2. Create a new team with:
   - **Name:** `copilot-studio-team`
   - **Description:** `Sample Copilot Studio team`
3. Create a channel named `new-properties` in the team. If the team creation screen offers a first-channel field, you can enter the name there.
4. If prompted to add members, select **Skip**

> **Note:** You can use an existing permitted team and channel instead. Use their actual names in the instructions and test prompt below.

### 3.2 Add the Teams MCP Server
1. Return to the agent in Copilot Studio and select the **Tools** tab
2. Select **+ Add a tool**, then select **Model Context Protocol**
3. Search for `Teams` and select the **Teams** MCP server
4. Create or select a connection and follow any sign-in and consent prompts
5. Select **Add and configure**
6. Review the available tools and turn off **Allow all**
7. Enable only the tools needed to list teams, list channels, and post a channel message:
   - `mcp_graph_teams_listTeams`
   - `mcp_graph_teams_listChannels`
   - `mcp_graph_teams_postChannelMessage`
8. Leave the other tools disabled and select **Save**

> **Note:** Tool names may differ in your environment. The connection must have access to the chosen team and permission to post in the channel. If the server is unavailable or consent is required, ask your administrator before continuing.

---

## Step 4: Post a Property Update

In this step, you will update the agent instructions, create a property listing, and ask the agent to post its details to the channel.

### 4.1 Update the Agent Instructions
1. Select the **Overview** tab
2. Select **Edit** in the **Instructions** section
3. Add the following to the existing instructions:
   ```text
   After a property listing has been created, use the Teams MCP tool to post its details to the new-properties channel in copilot-studio-team when the user requests an update. Use only confirmed property details and ask for any missing information. Do not claim that a message was posted unless the tool succeeds.
   ```
4. Select **Save**

> **Note:** Instructions guide the agent but do not guarantee that it will call a tool. This lab explicitly asks for the Teams post after the listing is created.

### 4.2 Create a Property Listing
1. Open the **Test your agent** pane and start a new test session
2. Enter:
   ```text
   I want to create a property listing.
   ```
3. When prompted, upload your sample property listing file
4. Complete any connection prompts and wait for the agent to confirm that the listing was created
5. Open **Real Estate Properties** in Microsoft Lists and verify the new item and its details

### 4.3 Post and Verify the Update
1. In the same test conversation, enter:
   ```text
   Post the confirmed details of the property I just created to the new-properties channel in copilot-studio-team.
   ```
2. If prompted, authorize the Teams MCP connection and confirm the post
3. Verify the agent reports a successful post. If it asks for missing details, provide the confirmed values from the new list item.
4. Open the `new-properties` channel in Teams and verify that the message contains the expected property details

> **Note:** If the agent cannot find or post to the channel, check the selected tools, connection permissions, team membership, and channel name. A confirmation in the conversation alone does not verify that the message was posted.

---

## Verification Checklist
- [ ] Generative orchestration enabled
- [ ] Agent authentication configured for the connected services
- [ ] Sample email sent to and visible in the connected mailbox
- [ ] **Mail MCP** added with message search and retrieval tools enabled
- [ ] Mail MCP write tools left disabled
- [ ] Sample email found and summarized in the Test pane
- [ ] Team and `new-properties` channel available
- [ ] **Teams MCP** added with team listing, channel listing, and channel posting tools enabled
- [ ] Agent instructions updated
- [ ] Property listing created and verified in Microsoft Lists
- [ ] Property update posted and verified in the Teams channel

---

## Summary and Key Takeaways

In this lab, you successfully:

1. ✅ Enabled generative orchestration for MCP tools
2. ✅ Configured Mail MCP to search and read a sample email
3. ✅ Configured Teams MCP to post to a channel
4. ✅ Created a property listing and verified it in Microsoft Lists
5. ✅ Requested and verified a Teams property update

### Key Learnings
- **MCP servers:** Extend an agent with tools for connected services
- **Generative orchestration:** Enables the agent to select MCP tools based on a user's request
- **Tool selection:** Enable only the actions needed for the scenario
- **Mail MCP:** Can search and retrieve messages available to its connection
- **Teams MCP:** Can locate a team and channel and post a message when permitted
- **Instructions:** Guide tool use but do not guarantee execution
- **Verification:** Check the mailbox, list, and Teams channel rather than relying only on the agent's response
- **Permissions:** Tool selection and the connected account's permissions both affect what the agent can do

---

## Lab Completion Badge

Congratulations! 🎉 You have completed the **Use MCP Servers in Copilot Studio** lab.

You now have hands-on experience with:
- Adding and restricting MCP tools
- Searching and reading sample email
- Connecting an agent to a Teams channel
- Testing and verifying an MCP-backed workflow

---

**End of Lab**
