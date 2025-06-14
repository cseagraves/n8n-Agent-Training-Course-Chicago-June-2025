# n8n Node Reference Library 📖

## Overview 📋

This n8n workflow serves as a comprehensive visual library and organizational template. Instead of performing a single, linear automated task, it acts as a "cheat sheet" by grouping a wide variety of common n8n nodes into logical categories. It is designed to help users quickly find, understand, and copy nodes for use in their own workflows.

## Purpose 🎯

This workflow template is designed to streamline development and serve as an educational tool. Its primary goals are:

* 📚 **Visual Learning:** Provides a clear, organized canvas to explore the types of nodes available in n8n.
* 🚀 **Accelerated Development:** Allows you to quickly find and copy pre-configured (though credential-less) nodes directly into your own projects.
* 🧩 **Categorical Organization:** Groups nodes by function (Triggers, Data Transformation, AI, etc.) to help understand their roles.
* 💡 **Best Practice Demonstration:** Shows a structured way to organize large or complex workflows using colored **Sticky Notes**.
* 🧪 **Rapid Prototyping:** Facilitates experimentation by providing a sandbox of ready-to-use components.

## Integrations & Tools 🔌

This library contains a wide array of nodes, categorized for clarity.

### 🟣 Triggers
*This section contains all trigger nodes that can start a workflow.*
* **🗓️ Calendly Trigger:** Starts the workflow when a Calendly event occurs.
* **📨 Email Trigger (IMAP):** Runs on receiving an email via IMAP.
* **📁 Google Drive Trigger:** Triggers on new or updated files in a Google Drive folder.
* **💰 Gumroad Trigger:** Activates upon a sale or new subscription in Gumroad.
* **💻 Local File Trigger:** Watches a local directory for file changes.
* **📝 On form submission:** Provides a web form that triggers the workflow when submitted.
* **⏰ Schedule Trigger:** Executes the workflow on a recurring schedule (e.g., every hour, every day).
* **🔗 Webhook:** Provides a unique URL to start the workflow from external services.
* **💬 When chat message received:** A specialized trigger for initiating chat-based AI agents.
* **🖱️ When clicking 'Test workflow':** A manual trigger for testing and development.

### 🔵 Data Transformation
*Nodes for manipulating, filtering, and converting data.*
* **🔢 Aggregate:** Combines multiple items into a single entry.
* **💻 Code:** Executes custom JavaScript code to manipulate data.
* **📄 Convert to File:** Transforms JSON data into a binary file (e.g., text, CSV).
* **🕰️ Date & Time:** Modifies and formats date and time values.
* **✏️ Edit Fields:** Add, edit, or remove fields from items.
* **🔍 Extract from File:** Pulls text content from files like PDFs.
* **🚦 Filter:** Allows items to pass only if they meet specific conditions.
* **🌐 HTML:** Extracts content from HTML using CSS selectors.
* **⚖️ Limit:** Restricts the number of items passing through.
* **🔄 Merge:** Combines data from two different input streams.
* **🚫 Remove Duplicates:** Filters out items that have identical values in specified fields.
* **🏷️ Rename Keys:** Renames JSON keys.
* **↔️ Sort:** Orders items based on specified key values.
* **↔️ Split Out:** Converts a single item with an array into multiple items.

### 🔴 Flow & Core
*Nodes for controlling workflow logic and core connectivity.*
* **⚙️ Execute Command:** Runs shell commands on the n8n server.
* **▶️ Execute Workflow:** Calls another n8n workflow.
* **🌐 HTTP Request:** Makes HTTP requests to external APIs.
* **❓ If:** Routes items down different branches based on conditions.
* **🔁 Loop Over Items:** Processes items one by one or in batches.
* **➡️ Respond to Webhook:** Sends a custom response to a webhook trigger.
* **⏳ Wait:** Pauses the workflow for a specified time or until a webhook is called.

### 🤖 AI Agents & LangChain
*Core components for building intelligent agents.*
* **🤖 AI Agent:** A powerful node that combines a Large Language Model (LLM) with tools to perform complex tasks.
* **🧠 Basic LLM Chain:** The simplest chain for calling an LLM.
* **🗂️ Information Extractor:** Extracts structured data (JSON) from unstructured text based on a schema.
* **❓ Question and Answer Chain:** Answers questions based on provided documents.
* **😊 Sentiment Analysis:** Determines the sentiment (positive, negative, neutral) of a text.
* **📑 Summarization Chain:** Creates a summary of long documents or text.
* **🏷️ Text Classifier:** Categorizes text based on predefined labels.
* **💾 Chat Memory Manager:** Manages conversational history for an AI agent.

### 🛠️ AI Tools
*Specialized tools that can be connected to an **AI Agent**.*
* **🧮 Calculator:** A tool for performing mathematical calculations.
* **📞 Call n8n Workflow Tool:** Allows an agent to execute another n8n workflow.
* **🔌 MCP Client:** A tool for interacting with the Mindful Contract Platform.
* **🔍 SerpAPI:** Provides search engine results to an agent.
* **📚 Wikipedia:** A tool for querying Wikipedia.
* **🐺 Wolfram Alpha:** Gives the agent access to computational knowledge.
* **✉️ Send Email / gmailTool:** Tools for sending and managing emails.
* **📅 googleCalendarTool:** A tool for interacting with Google Calendar.
* **📄 googleDocsTool / googleSheetsTool:** Tools for reading from and writing to Google Docs and Sheets.
* **🛢️ Postgres / Redis:** Tools to query PostgreSQL or Redis databases.

### 💾 Vector Memory
*Tools for providing AI Agents with long-term memory and knowledge retrieval.*
* **🧠 In-Memory Vector Store:** Stores document embeddings in memory for the duration of the workflow execution.
* **🌲 Pinecone Vector Store:** Connects to a Pinecone index for persistent vector storage.
* **🐘 Postgres PGVector Store:** Uses a PostgreSQL database with the pgvector extension.
* **☁️ Supabase Vector Store:** Connects to a Supabase project for vector storage.
* **🔍 Answer questions with a vector store:** A dedicated tool for querying a vector store.

### 🧩 Miscellaneous AI Tools
*Essential utilities for AI workflows.*
* **🤖 Models:** Anthropic, Google Gemini, and OpenAI chat models.
* **↪️ Embeddings:** OpenAI and Google Gemini models for creating text embeddings.
* **💾 Memory:** Persistent chat memory options using Postgres or Redis.
* **🧾 Parsers:** Tools to structure and validate output from LLMs.

### 🔌 App Actions
*Nodes for interacting with external apps and services.*
* **Bitly, Bluesky, Dropbox, ElevenLabs, Gmail, Google Calendar, Google Docs, Google Sheets, Perplexity, Pushbullet, Reddit, X (Twitter), YouTube, RSS Read.**

## Setup Instructions ⚙️

This workflow is a template and does not require traditional setup for execution. Follow these steps to use it as a library:

1.  **📥 Import the Workflow:** Import the `workflow.json` file into your n8n instance.
2.  **🧭 Explore the Canvas:** Navigate the different colored sections to see how nodes are grouped. Zoom in to read the descriptions on the **Sticky Notes**.
3.  **🔑 Configure Credentials:** Most nodes require credentials (e.g., API keys, OAuth2). To test or use a node, you must first configure its credentials in your n8n instance.
    * Click on a node (e.g., **Gmail App**).
    * In the properties panel on the right, select your pre-configured credential from the dropdown or create a new one.
4.  **💾 Save Your Changes:** Once you've added credentials to a node you wish to test, save the workflow.

## Usage Guidelines 📖

This workflow is **not meant to be executed** from start to finish. It is a visual repository.

* **✂️ Copying a Single Node:**
    1.  Click on the node you want to use (e.g., **HTTP Request**).
    2.  Copy it (`Ctrl+C` or `Cmd+C`).
    3.  Open the workflow where you want to use it.
    4.  Paste it (`Ctrl+V` or `Cmd+V`).

* **📝 Copying a Group of Nodes:**
    1.  Hold down the `Shift` key.
    2.  Click and drag to draw a selection box around the nodes you want to copy.
    3.  Copy and paste them into your target workflow.

* **Example Use Case:**
    * You need to read a Google Sheet and then send an email for each row.
    * Navigate to the **#Triggers** section and copy the **Google Sheets Trigger**.
    * Navigate to the **#App Actions** section and copy the **Gmail App** node.
    * Paste both into your new workflow, configure them, and connect them.

## Best Practices ✅

* **🔒 Credential Management:** Never save credentials directly in the workflow. Always use n8n's built-in credential management system. This template is designed to be shared, and hardcoded secrets are a major security risk.
* **✨ Maintain Organization:** If you add new nodes to your version of this library, use **Sticky Notes** and consistent coloring to keep it organized.
* **🧪 Isolate and Test:** When you copy a node into a new workflow, test it in isolation with a **Manual Trigger** to ensure it's configured correctly before building out complex logic.
* **🗑️ Clean Unused Nodes:** For any workflow you build *from* this library, delete any nodes that are not part of your final logic to improve clarity and performance.

## Troubleshooting ❓

* **❌ Error: "Authentication failed" or "Missing credentials"**
    * **Solution:** This is the most common issue. You have copied a node but have not selected a valid credential for it in the node's properties panel. Ensure you have created and selected the appropriate API key or OAuth2 connection for that service in your n8n instance.

* **🤔 Issue: "The workflow doesn't do anything when I press 'Test workflow'"**
    * **Solution:** This is by design. The workflow is a library, not a single automated process. There is no trigger connected to a final action. Use it to copy nodes into *other* workflows that have a defined start-to-finish logic.

* **📄 Error: "Cannot read property 'json' of undefined"**
    * **Solution:** This often happens when a node expects input data but isn't receiving it. When testing a copied node, make sure you are feeding it the correct data structure. You can use an **Edit Fields** node right after a manual trigger to create mock data for testing.

## Advanced Customization 🔧

* **🧩 Create Custom Toolsets:** Group nodes into new colored sections for your specific needs. For example, create a "Financial Analysis" section with stock data APIs, data transformation nodes, and an AI agent.
* **🤖 Build a New Agent:**
    1.  Copy the **AI Agent** node.
    2.  Copy a **Chat Model** (like **OpenAI Chat Model**).
    3.  Copy various **AI Tools** you need (like **SerpAPI** and **Calculator**).
    4.  Connect the model and tools to the **AI Agent** to give it new capabilities.
* **⚠️ Enhance Error Handling:** For workflows you build, add **If** nodes or use the "Continue on Fail" setting to handle potential errors gracefully, preventing your workflow from stopping unexpectedly.

## Performance Optimization ⚡

While this library itself doesn't have performance considerations, workflows built from it do.

* **⚙️ Filter Early:** Use **Filter** or **If** nodes as early as possible in your workflow to reduce the number of items that need to be processed by subsequent nodes.
* **💾 Use Vector Stores for AI:** When building AI agents that need to reference large amounts of information, use a **Vector Store** (like Pinecone or Supabase) instead of passing large text files in the workflow itself. This is significantly faster and more scalable.
* **💨 Batch Processing:** For workflows that handle thousands of items, use the `batch` settings in nodes like **Loop Over Items** to process data in chunks, reducing memory consumption.

## Security Considerations 🔒

* **🔐 Credentials:** **Do not hardcode API keys, passwords, or any secrets** in `code` blocks or node parameters. Always use the encrypted credentials store provided by n8n.
* **💻 Execute Command Node:** Be extremely cautious with the **Execute Command** node. It can run any command on the server where n8n is hosted. Only use it if you fully understand the security implications and have secured your n8n instance.
* **📦 Data Privacy:** When using external AI services (like OpenAI, Google Gemini) or other third-party apps, be aware of their data privacy policies. Avoid sending sensitive personal, financial, or proprietary information unless you are certain it complies with your organization's security standards.
* **📝 Audit and Review:** Regularly review workflows that handle sensitive data to ensure they are not exposing information unintentionally, for example, in a **Respond to Webhook** node.