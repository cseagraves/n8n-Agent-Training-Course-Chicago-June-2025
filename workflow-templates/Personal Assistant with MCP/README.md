# 🤖 n8n Personal Assistant with MCP

## 📋 Overview

This workflow creates a powerful personal assistant AI agent that integrates with Google services (Calendar, Gmail, Sheets) using the Multi-Channel Provider (MCP) architecture. The agent can handle email management, calendar scheduling, and contact management through natural language conversations.

## 🎯 Purpose

The Personal Assistant with MCP workflow serves as a virtual assistant that can:
- 📅 Manage your calendar (create, find, update events)
- 📧 Handle email correspondence (search, draft emails)
- 👥 Maintain contact information in Google Sheets
- 💬 Provide a seamless chat interface for all these operations

## 🔌 Integrations & Tools

### Core Components
- **🔄 MCP Client & Server**: Enables multi-channel communication with the agent
- **💬 Chat Trigger**: Entry point for user interactions
- **🧠 Window Buffer Memory**: Maintains conversation context
- **🤖 OpenRouter LLM**: Powers the AI using GPT-4.1-mini model

### Google Service Integrations
- **📅 Google Calendar**: Create, find, and update calendar events
- **📧 Gmail**: Search emails and draft new messages
- **📊 Google Sheets**: Store and manage contact information

## ⚙️ Setup Instructions

1. **MCP Configuration**:
   - Set up the MCP Server Trigger node
   - Copy the generated URL to the MCP Client node's `sseEndpoint` parameter
   - Configure your preferred notification channel (Telegram, Gmail, etc.)

2. **Google API Credentials**:
   - Set up Google OAuth2 credentials with access to:
     - Gmail API
     - Google Calendar API
     - Google Sheets API
   - Ensure proper scopes are enabled for each service

3. **Google Sheets Setup**:
   - Create a "Contacts" spreadsheet in Google Drive
   - Set up columns for contact information (name, email, phone, company, etc.)
   - Update the spreadsheet ID in the Google Sheets nodes

4. **LLM Configuration**:
   - Add your OpenRouter API credentials
   - Optionally adjust the model to your preferred LLM

## 💡 Usage Tips & Best Practices

### 📅 Calendar Management
- Use natural language for date and time references
- Combine multiple calendar operations in single requests
- Include all necessary event details (title, time, participants)

**Example Commands:**
- "Schedule a meeting with John tomorrow at 2pm for 30 minutes"
- "Find all my meetings next week"
- "Update my 3pm meeting today to start at 4pm instead"

### 📧 Email Handling
- Be specific about email search criteria
- Provide clear instructions for email drafting
- Include recipient information when necessary

**Example Commands:**
- "Find the last 5 emails from Sarah about the project proposal"
- "Draft an email to John explaining I need to reschedule our meeting"
- "Search for emails with attachments from last week"

### 👥 Contact Management
- Provide complete contact information when adding new contacts
- Use specific identifiers when searching for contacts
- Be explicit about which fields to update

**Example Commands:**
- "Add a new contact: John Smith, email john@example.com, phone 555-1234"
- "Find contact information for Sarah Johnson"
- "Update Rick's email to rick@newcompany.com"

### 🔄 Multi-Operation Commands
The true power of this agent is in combining multiple operations:

**Example:**
- "Find the contact for John at ABC Corp, send him an email scheduling a meeting for next Wednesday at 9AM, and update his company information to XYZ Corp"

## ❓ Troubleshooting

- **🔌 MCP Connection Issues**: Verify the SSE endpoint URL is correctly copied from the MCP Server Trigger node
- **🔑 Google API Errors**: Check credential scopes and permissions
- **📊 Missing Contact Data**: Verify Google Sheet structure matches expected format
- **📅 Calendar Scheduling Problems**: Ensure date/time formats are recognizable

## 🔧 Customization Options

- **🧩 Add Additional Tools**: Expand functionality with other API integrations
- **💬 Enhance System Prompt**: Modify the agent's behavior and capabilities
- **⚠️ Add Error Handling**: Implement notification for failed operations
- **🎨 Custom Output Formatting**: Adjust how the agent presents information

## 🔒 Security Considerations

- Ensure Google API credentials are properly secured
- Limit access to the workflow to authorized users
- Consider implementing user authentication for the chat interface
- Regularly review and revoke unused API access

## ⚡ Performance Optimization

- Adjust memory window size based on conversation complexity
- Consider using a more powerful LLM for complex tasks
- 💾 Implement caching for frequently accessed information
- Use batch operations when processing multiple items 