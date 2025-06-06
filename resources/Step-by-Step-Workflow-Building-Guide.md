# Step-by-Step Workflow Building Guide

**n8n AI Agent Training Course - June 14th, 2025**  
**Building Real Estate AI Workflows from Foundation to Advanced**

---

## Table of Contents

1. [Workflow Building Fundamentals](#1-workflow-building-fundamentals)
   - [Core n8n Concepts](#core-n8n-concepts)
   - [The Anatomy of a Real Estate AI Agent](#the-anatomy-of-a-real-estate-ai-agent)
   - [Essential Node Types for Real Estate](#essential-node-types-for-real-estate)
2. [Project 1: Lead Nurture Assistant](#2-project-1-lead-nurture-assistant)
3. [Project 2: Market Intelligence Agent](#3-project-2-market-intelligence-agent)
4. [Project 3: Contract Analysis with RAG](#4-project-3-contract-analysis-with-rag)
5. [Project 4: Social Media Automation](#5-project-4-social-media-automation)
6. [Project 5: Multi-Agent System (MAS) Orchestration](#6-project-5-multi-agent-system-mas-orchestration)
7. [Testing and Debugging Best Practices](#7-testing-and-debugging-best-practices)
8. [Deployment and Scaling Strategies](#8-deployment-and-scaling-strategies)

---

## 1. Workflow Building Fundamentals

### Core n8n Concepts

- **Nodes:** The fundamental building blocks of any n8n workflow. Each node performs a specific function.
  - **Trigger Nodes:** These initiate a workflow. They can be activated by a schedule (e.g., every morning), a webhook call from an external service (like a new lead from your website), or manually for testing.
  - **Regular Nodes:** These are the workhorses of your workflow, performing actions like making API calls (`HTTP Request`), manipulating data (`Set`, `Function`), or applying conditional logic (`IF`, `Switch`).
  - **AI Nodes:** The "brain" of modern workflows. This category includes nodes for interacting with large language models (`Chat Model`), leveraging pre-built agentic logic (`AI Agent`), and connecting to knowledge bases (`Vector Store`).

- **Connections:** The "wires" that link nodes together, defining the path that data follows. In n8n, you'll encounter several types of connection points, each with a specific purpose (e.g., `Main` for data, `AI` for models, `Memory` for context, `Tools` for agent capabilities).

- **Data Structure (JSON):** Data in n8n flows between nodes in a structured format called JSON (JavaScript Object Notation). It's organized into "items," which are like individual records. Understanding this structure is key to manipulating data effectively.
  ```json
  {
    "id": 123,
    "lead_source": "Website Form",
    "contact_info": {
      "name": "Jane Doe",
      "email": "jane.doe@example.com"
    },
    "inquiry": "I'm interested in 3-bedroom homes in the downtown area."
  }
  ```

### The Anatomy of a Real Estate AI Agent

An AI Agent is more than a simple automation; it's a system designed to perceive its environment, make decisions, and take actions to achieve a specific goal. In n8n, you'll build agents with these core components:

- **Perception Module:** How the agent receives information. This is often your **Trigger Node** (e.g., a Webhook capturing a new lead) or a node that fetches data (e.g., an `HTTP Request` node checking an MLS feed).
- **Reasoning/Cognition Module:** The "brain" of your agent. This is typically the **AI Agent** or **Chat Model** node, where you use an LLM (like GPT-4o or Claude 3.5) to analyze the perceived information and decide on the next action. This is where the magic of prompt engineering comes in.
- **Action Module:** The component that executes the decision. This could be any number of n8n nodes: sending an email (`Gmail` node), updating a client record (`Google Sheets` or CRM node), or calling another API (`HTTP Request` node).
- **Knowledge Base/Memory:** Where the agent stores information for context. This can range from short-term memory (data passed between nodes) to long-term memory managed in a database like **Pinecone** or **Supabase**, which is essential for advanced patterns like Retrieval-Augmented Generation (RAG).

### Essential Node Types for Real Estate

While n8n has hundreds of integrations, a handful of nodes will form the backbone of nearly every real estate workflow you build:

1.  **Webhook Trigger:** The primary way to capture real-time events, such as new leads from your website, CRM updates, or form submissions.
2.  **AI Agent Node:** The core of your intelligent workflows. This is where you'll define the agent's goals, provide it with tools, and connect it to a powerful language model.
3.  **HTTP Request Node:** Your universal connector. Use this to interact with any third-party service that has an API, such as MLS data providers, marketing platforms, or other real estate tech tools.
4.  **Set Node:** A simple but powerful node for structuring and cleaning data. Use it to rename fields, set default values, and prepare data for the next step in your workflow.
5.  **IF Node:** The fundamental tool for conditional logic. It allows your workflow to take different paths based on specific criteria (e.g., "Is the lead's budget over $500k?").
6.  **Google Sheets Node:** An incredibly versatile tool for real estate. Use it as a simple database for tracking leads, managing properties, or storing data for your automations.
7.  **Email Node (Gmail, etc.):** Essential for communication. Send automated follow-ups, internal notifications, and client updates directly from your workflows.
8.  **Vector Store Nodes (Pinecone, Supabase, etc.):** Critical for building agents with memory (RAG). These nodes allow your agent to retrieve relevant information from your own knowledge base (e.g., property details, market reports) to provide accurate, context-aware responses.

---

## 2. Project 1: Lead Nurture Assistant

**Goal:** Automatically qualify and nurture incoming leads with personalized responses

### Step 1: Set Up the Trigger

1. **Add Webhook Trigger**
   - Drag "Webhook" node to canvas
   - Set HTTP Method: `POST`
   - Copy webhook URL for later use

2. **Configure Webhook**
   - Path: `/lead-capture`
   - Response Mode: `Respond to Webhook`
   - Response Data: `First Incoming Item`

### Step 2: Data Processing

1. **Add Set Node**
   - Connect to Webhook trigger
   - Name: "Clean Lead Data"
   - Configure fields:
     ```
     name: {{ $json.body.name }}
     email: {{ $json.body.email }}
     phone: {{ $json.body.phone }}
     property_type: {{ $json.body.property_type }}
     budget: {{ $json.body.budget }}
     timeline: {{ $json.body.timeline }}
     message: {{ $json.body.message }}
     timestamp: {{ $now }}
     ```

### Step 3: Lead Qualification

1. **Add AI Agent Node**
   - Connect to Set node
   - Name: "Lead Qualifier"

2. **Configure AI Agent**
   - Prompt: `Take from previous node automatically`
   - System Message:
     ```
     You are a real estate lead qualification specialist. 
     Analyze the lead data and provide:
     1. Lead score (1-10)
     2. Qualification status (Hot, Warm, Cold)
     3. A brief (1-2 sentence) summary of the lead's main interests.
     4. Recommended next action for the agent.
     
     Base your analysis on the lead's budget, timeline, desired property type, and the content of their message.
     Respond ONLY with a valid JSON object.
     ```

3. **Add Chat Model**
   - Connect OpenAI Chat Model
   - Model: `gpt-4o-mini`
   - Use your OpenRouter credentials

### Step 4: Conditional Routing

1. **Add IF Node**
   - Connect to AI Agent
   - Name: "Route by Lead Score"
   - Condition: `{{ $json.lead_score >= 7 }}`

### Step 5: High-Value Lead Path

1. **Add Set Node** (True branch)
   - Name: "Prepare Hot Lead Response"
   - Configure:
     ```
     lead_status: "Hot"
     priority: "High"
     response_template: "immediate_followup"
     assign_to: "senior_agent"
     ```

2. **Add Email Node**
   - Connect to Set node
   - To: `{{ $('Clean Lead Data').item.json.email }}`
   - Subject: `Thank you for your interest - Let's schedule a call!`
   - Body: Use personalized template

### Step 6: Standard Lead Path

1. **Add Set Node** (False branch)
   - Name: "Prepare Standard Response"
   - Configure:
     ```
     lead_status: "Warm"
     priority: "Standard"
     response_template: "nurture_sequence"
     assign_to: "team_pool"
     ```

2. **Add Email Node**
   - Personalized nurture email template

### Step 7: CRM Integration

1. **Add Google Sheets Node**
   - Connect to both email paths
   - Operation: `Append`
   - Spreadsheet: Your lead tracking sheet
   - Map all lead data and qualification results

### Step 8: Testing

1. **Test Webhook**
   - Use Postman or curl to send test data
   - Verify data flow through all nodes
   - Check email delivery and sheet updates

---

## 3. Project 2: Market Intelligence Agent

**Goal:** Monitor market trends and generate automated reports

### Step 1: Schedule Trigger

1. **Add Schedule Trigger**
   - Interval: `Every day at 8:00 AM`
   - Timezone: Your local timezone

### Step 2: Market Data Collection

1. **Add HTTP Request Node**
   - Name: "Fetch MLS Data"
   - Method: `GET`
   - URL: Your MLS API endpoint
   - Headers: Include authentication

2. **Add Tavily Search Node**
   - Name: "Market News Search"
   - Query: `real estate market trends {{ $now.format('YYYY') }}`
   - Max Results: `10`

### Step 3: Data Analysis

1. **Add AI Agent Node**
   - Name: "Market Analyst"
   - System Message:
     ```
     You are a real estate market analyst. Analyze the provided 
     MLS data and news articles to create a comprehensive market report.
     
     Include:
     1. Key market trends
     2. Price movements
     3. Inventory levels
     4. Market predictions
     5. Investment opportunities
     
     Format as a professional market report.
     ```

2. **Connect Tools**
   - Calculator Tool for price analysis
   - HTTP Request Tool for additional data

### Step 4: Report Generation

1. **Add Google Docs Node**
   - Operation: `Create Document`
   - Title: `Market Report - {{ $now.format('YYYY-MM-DD') }}`
   - Content: AI-generated report

### Step 5: Distribution

1. **Add Email Node**
   - To: Your client list
   - Subject: `Weekly Market Intelligence Report`
   - Attach generated document

---

## 4. Project 3: Contract Analysis with RAG

**Goal:** Analyze real estate contracts using AI with document knowledge base

### Step 1: Document Upload Trigger

1. **Add Webhook Trigger**
   - Path: `/contract-upload`
   - Accept file uploads

### Step 2: Document Processing

1. **Add Extract from File Node**
   - Extract text from PDF contracts
   - Support multiple formats

2. **Add Text Splitter Node**
   - Chunk size: `1000`
   - Overlap: `200`
   - Split method: `Recursive Character`

### Step 3: Vector Storage

1. **Add Embeddings Node**
   - Model: OpenAI Embeddings
   - Connect to text chunks

2. **Add Pinecone Node**
   - Operation: `Insert`
   - Index: `contract-knowledge`
   - Store embeddings with metadata

### Step 4: Contract Analysis

1. **Add AI Agent Node**
   - Name: "Contract Analyzer"
   - System Message:
     ```
     You are a real estate contract specialist. Analyze contracts for:
     1. Key terms and conditions
     2. Potential risks or issues
     3. Missing clauses
     4. Compliance with local laws
     5. Recommendations for negotiation
     
     Use the knowledge base to reference similar contracts and best practices.
     ```

2. **Add Vector Store Tool**
   - Connect to Pinecone
   - Enable retrieval for context

### Step 5: Report Generation

1. **Add Set Node**
   - Structure analysis results
   - Include risk scores and recommendations

2. **Add Google Docs Node**
   - Generate detailed analysis report
   - Include highlighted sections

---

## 5. Project 4: Social Media Automation

**Goal:** Automatically post property listings and market content

### Step 1: Content Trigger

1. **Add Google Sheets Trigger**
   - Monitor new property listings
   - Trigger on row addition

### Step 2: Content Generation

1. **Add AI Agent Node**
   - Name: "Social Media Creator"
   - System Message:
     ```
     Create engaging social media content for real estate listings.
     Generate:
     1. Compelling property descriptions
     2. Relevant hashtags
     3. Call-to-action text
     4. Platform-specific variations (Facebook, Instagram, LinkedIn)
     
     Keep content professional yet engaging.
     ```

### Step 3: Image Processing

1. **Add HTTP Request Node**
   - Fetch property images
   - Resize for social platforms

### Step 4: Multi-Platform Posting

1. **Add Facebook Node**
   - Post to business page
   - Include images and description

2. **Add LinkedIn Node**
   - Share to company page
   - Professional tone

3. **Add Instagram Node** (via Facebook API)
   - Visual-focused content
   - Story and feed posts

### Step 5: Performance Tracking

1. **Add Google Sheets Node**
   - Log all posts with timestamps
   - Track engagement metrics

---

## 6. Project 5: Multi-Agent System (MAS) Orchestration

**Goal:** Coordinate multiple specialized AI agents to handle a complex real estate inquiry, demonstrating advanced workflow orchestration.

### Step 1: Master Coordinator Agent

1. **Add AI Agent Node**
   - Name: "Master Coordinator"
   - System Message:
     ```
     You are the master coordinator for a real estate AI system.
     Your job is to analyze incoming client requests and delegate tasks to the appropriate specialist agents.
     
     The available agents are:
     1. **Lead Qualification Agent:** Determines if a new contact is a viable lead.
     2. **Property Research Agent:** Finds and analyzes specific property details from the MLS.  
     3. **Market Analysis Agent:** Provides trends, stats, and analysis for a given neighborhood or zip code.
     4. **Client Communications Agent:** Drafts emails and messages to clients.
     
     Based on the user's request, decide which agent (or sequence of agents) is needed to fulfill the request.
     Respond with a JSON object indicating the required agent and the specific query for that agent.
     Example: {"agent_needed": "Property Research", "query": "Find details for MLS #12345"}
     ```

### Step 2: Specialist Agent Sub-Workflows

(Note: In a real system, these would be separate, callable workflows. For this guide, we'll represent them as branches.)

1. **Create Specialist Branches using a Switch Node**
   - Connect a `Switch` node to the `Master Coordinator`.
   - Configure routing based on the `agent_needed` output from the coordinator.
   - Create an output for each specialist agent (e.g., "Lead Qualification", "Property Research").

2. **Build out each Specialist Agent Branch**
   - **Property Research Branch:**
     - Add an `HTTP Request` node to call your MLS API.
     - Add an `AI Agent` node to summarize the property details.
   - **Market Analysis Branch:**
     - Add `HTTP Request` nodes to fetch market data (e.g., from a real estate data API).
     - Add an `AI Agent` to synthesize the data into a market summary.
   - **Client Communications Branch:**
     - Add an `AI Agent` node with a system prompt focused on drafting professional, client-facing emails.

### Step 3: Agent Coordination and Response

1. **Merge Responses**
   - Use a `Merge` node to bring the outputs from the different specialist branches back into a single flow.

2. **Synthesize Final Response**
   - Add a final `AI Agent` node.
   - Name: "Response Synthesizer"
   - System Message:
     ```
     You have received analysis from one or more specialist AI agents.
     Your task is to synthesize all the information into a single, coherent, and helpful response for the original client query.
     Ensure the final output is well-formatted and directly addresses the user's request.
     ```

### Step 4: Execute Final Action

1.  **Add an Email or Chat Node**
    -  Send the final, synthesized response back to the client.

---

## 7. Testing and Debugging Best Practices

### Testing Strategies

1. **Unit Testing**
   - Test individual nodes
   - Use manual triggers
   - Verify data transformations

2. **Integration Testing**
   - Test complete workflows
   - Use realistic test data
   - Check external API responses

3. **Error Handling**
   - Use the `Error Trigger` node to create dedicated error-handling workflows.
   - Implement fallback options (e.g., if an API fails, send a notification instead of stopping).
   - Log errors to a Google Sheet or database for later review.

### Debugging Tools in n8n

1. **Node Execution Data & Data Pinning**
   - View input/output for each node
   - Check data transformations
   - Identify bottlenecks
   - "Pin" the output of a node to avoid re-executing upstream nodes during testing, which saves time and API calls.

2. **Workflow Execution Logs**
   - In the n8n UI, go to "Executions" to see a history of all workflow runs.
   - This is crucial for debugging scheduled or webhook-triggered workflows that fail in production.
   - You can inspect the data at each step of a failed execution.

3. **Test Webhooks**
   - Use webhook.site for testing
   - Verify payload structure
   - Debug authentication issues

### Common Issues and Solutions

**Issue:** AI Agent not responding
**Solution:** Check model credentials and prompt format

**Issue:** Data not flowing between nodes
**Solution:** Verify node connections and data structure

**Issue:** API rate limits exceeded
**Solution:** Add delays and implement retry logic

**Issue:** Memory issues with large datasets
**Solution:** Use pagination and batch processing

---

## 8. Deployment and Scaling Strategies

### Environment Management

1. **Development Environment**
   - Use test credentials
   - Limited external connections
   - Extensive logging enabled

2. **Production Environment**
   - Use production-level credentials.
   - Optimize workflows for performance (e.g., fewer nodes, batch processing).
   - Enable robust error handling and monitoring.

### Security Considerations

1. **Credential Management**
   - Use n8n credential system
   - Regular key rotation
   - Principle of least privilege

2. **Data Protection**
   - Encrypt sensitive data
   - Comply with privacy regulations
   - Implement data retention policies
   - Be mindful of handling Personally Identifiable Information (PII).
   - Ensure compliance with relevant data privacy regulations (e.g., GDPR, CCPA).
   - Implement data retention policies to automatically delete old data.

### Monitoring and Maintenance

1. **Performance Monitoring**
   - Track execution times
   - Monitor resource usage
   - Set up alerts for failures

2. **Regular Updates & Audits**
   - Keep n8n and its nodes updated to the latest versions.
   - Periodically review and optimize workflows for efficiency.
   - Audit AI prompts and logic to ensure they remain aligned with business goals.

### Scaling Your Workflows

1. **Workflow Optimization**
   - For high-volume tasks, process data in batches using the `Loop Over Items` node.
   - Use caching for frequently accessed data that doesn't change often (e.g., property details) to reduce API calls.
   - Offload heavy computation to external services (like Google Cloud Functions or Supabase Edge Functions) and use n8n for orchestration.

2. **n8n Instance Scaling**
   - For self-hosted instances, monitor server resource usage (CPU, memory) and scale up as needed.
   - For very high throughput, explore n8n's "Queue Mode" to distribute executions across multiple worker instances.

---

## Conclusion

This guide provides the foundation for building sophisticated AI-powered real estate workflows. Start with simple projects and gradually add complexity as you become more comfortable with n8n and AI agent concepts.

**Key Success Factors:**
1. Start simple and iterate
2. Test thoroughly at each step
3. Monitor performance and optimize
4. Maintain good documentation
5. Keep security and compliance in mind

**Next Steps:**
1. Choose your first project based on business priority
2. Set up all required credentials and connections
3. Build and test in development environment
4. Deploy to production with proper monitoring
5. Gather feedback and iterate

---

*This guide serves as your roadmap for the hands-on workshop. We'll build these workflows together, providing practical experience with each concept and technique.* 