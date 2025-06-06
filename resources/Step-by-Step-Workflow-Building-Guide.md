# Step-by-Step Workflow Building Guide

**n8n AI Agent Training Course - Real Estate Automation**  
*Detailed Instructions for Building Core Real Estate Workflows*

---

## Table of Contents

1. [General Workflow Building Principles](#general-workflow-building-principles)
2. [Lead Nurture Assistant Workflow](#lead-nurture-assistant-workflow)
3. [Market Intelligence Agent](#market-intelligence-agent)
4. [Contract Analysis Agent (RAG)](#contract-analysis-agent-rag)
5. [Social Media Automation](#social-media-automation)
6. [Multi-Agent System Architecture](#multi-agent-system-architecture)
7. [Testing and Debugging Guidelines](#testing-and-debugging-guidelines)
8. [Performance Optimization](#performance-optimization)

---

## General Workflow Building Principles

### Before You Start
1. **Clear Objective:** Define exactly what the workflow should accomplish
2. **Data Flow:** Map out inputs, processing steps, and outputs
3. **Error Scenarios:** Consider what could go wrong and plan error handling
4. **Testing Strategy:** Plan how you'll test each component

### Workflow Design Best Practices

#### 1. **Start Simple, Then Expand**
- Begin with core functionality
- Test thoroughly before adding complexity
- Use modular design for easier maintenance

#### 2. **Use Descriptive Naming**
- **Nodes:** "Extract Lead Email" instead of "Set1"
- **Variables:** "leadScore" instead of "var1"
- **Workflows:** "Lead Qualification v2" instead of "Workflow Copy"

#### 3. **Implement Error Handling**
- Add IF nodes to check for required data
- Use Try/Catch patterns with Error Trigger nodes
- Include fallback paths for critical operations

#### 4. **Document Your Work**
- Use Sticky Notes to explain complex logic
- Color-code nodes by function (Blue=Input, Green=Processing, Orange=Decision, Red=Error, Purple=Output)
- Include version notes and change logs

### n8n Node Categories

#### Core Node Types
- **Trigger Nodes:** Start workflows (Webhook, Schedule, Manual)
- **Regular Nodes:** Process data (HTTP Request, Set, Function)
- **AI Nodes:** LangChain integrations (Agent, Chat Model, Tools)
- **Connector Nodes:** Third-party integrations (Google, Slack, etc.)

#### Data Flow Concepts
- **Items:** Individual data records flowing through workflow
- **Connections:** Lines connecting nodes showing data flow
- **Expressions:** Dynamic values using `{{ }}` syntax
- **Variables:** Workflow-level data storage

---

## Lead Nurture Assistant Workflow

**Objective:** Automatically capture, qualify, and nurture real estate leads through personalized follow-up sequences.

### Prerequisites
- Website form or lead capture mechanism
- Email service (Gmail or SendGrid)
- CRM integration (optional but recommended)
- Calendar service for appointment scheduling

### Step 1: Lead Capture Setup

#### 1.1 Create Webhook Trigger
```
Node: Webhook
Method: POST
Response Mode: Last Node
Authentication: None (for now)
```

**Expected Input Format:**
```json
{
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "555-123-4567",
  "message": "Interested in 3BR home under $400k",
  "source": "website"
}
```

#### 1.2 Add Data Validation
```
Node: IF
Condition: {{ $json.email && $json.name }}
Continue If: true
```

### Step 2: Lead Qualification with AI

#### 2.1 Set Up AI Agent for Lead Analysis
```
Node: AI Agent
Prompt Source: Define in Node
System Message: "You are a real estate lead qualification specialist. Analyze the lead's message and extract:
1. Intent (buying/selling/renting/investing)
2. Budget range (if mentioned)
3. Property type preference
4. Timeline urgency (immediate/3-6 months/planning)
5. Location preference
6. Lead quality score (1-10)

Respond in JSON format."
```

#### 2.2 Connect OpenAI Chat Model
```
Node: OpenAI Chat Model (sub-node)
Model: gpt-4o-mini
Temperature: 0.3
Max Tokens: 500
```

#### 2.3 Extract AI Analysis Results
```
Node: Set
Mode: Manual
Fields:
- intent: {{ $json.output.intent }}
- budget: {{ $json.output.budget }}
- timeline: {{ $json.output.timeline }}
- leadScore: {{ $json.output.leadScore }}
```

### Step 3: Lead Scoring and Routing

#### 3.1 Calculate Combined Lead Score
```
Node: Function
Code:
const baseScore = $input.item(0).json.leadScore || 5;
const sourceBonus = $input.item(0).json.source === 'referral' ? 2 : 0;
const urgencyBonus = $input.item(0).json.timeline === 'immediate' ? 3 : 
                    $input.item(0).json.timeline === '3-6 months' ? 1 : 0;

const finalScore = Math.min(10, baseScore + sourceBonus + urgencyBonus);

return {
  json: {
    ...$.item(0).json,
    finalLeadScore: finalScore,
    priority: finalScore >= 8 ? 'hot' : finalScore >= 6 ? 'warm' : 'cold'
  }
};
```

#### 3.2 Route Based on Score
```
Node: Switch
Mode: Expression
Value: {{ $json.priority }}
Routing:
- hot: Immediate response path
- warm: Standard nurture sequence
- cold: Long-term nurture sequence
```

### Step 4: Immediate Response (Hot Leads)

#### 4.1 Send Instant Email Response
```
Node: Gmail
Operation: Send Email
To: {{ $json.email }}
Subject: "Immediate Response - Your Property Inquiry"
Body: "Hi {{ $json.name }},

Thank you for your interest! Based on your inquiry about {{ $json.message }}, I'd love to discuss your needs immediately.

I have some properties that might be perfect for you. When would be a good time for a brief call today or tomorrow?

Best regards,
[Your Name]"
```

#### 4.2 Create Calendar Event Reminder
```
Node: Google Calendar
Operation: Create Event
Summary: "Follow up with {{ $json.name }} - Hot Lead"
Description: "Lead Details: {{ $json.message }}"
Start Time: {{ $now.plus({ hours: 2 }) }}
Duration: 30 minutes
```

#### 4.3 Send Internal Notification
```
Node: Slack/Telegram
Message: "🔥 HOT LEAD ALERT 🔥
Name: {{ $json.name }}
Email: {{ $json.email }}
Score: {{ $json.finalLeadScore }}/10
Intent: {{ $json.intent }}
Message: {{ $json.message }}"
```

### Step 5: Standard Nurture Sequence (Warm Leads)

#### 5.1 Initial Response Email
```
Node: Gmail
Delay: 0 minutes
Subject: "Thanks for your interest - Let's find your perfect home!"
Body: Personalized template based on intent and preferences
```

#### 5.2 Follow-up Email Series
```
Node: Wait (2 days)
↓
Node: Gmail
Subject: "Properties matching your criteria"
Body: Include property suggestions based on their preferences
↓
Node: Wait (5 days)
↓
Node: Gmail
Subject: "Market insights for your area"
Body: Local market information and trends
```

### Step 6: CRM Integration

#### 6.1 Create Contact in CRM
```
Node: HTTP Request (for CRM API)
Method: POST
URL: https://your-crm.com/api/contacts
Headers:
  Authorization: Bearer {{ $credentials.crmToken }}
  Content-Type: application/json
Body:
{
  "name": "{{ $json.name }}",
  "email": "{{ $json.email }}",
  "phone": "{{ $json.phone }}",
  "source": "{{ $json.source }}",
  "lead_score": {{ $json.finalLeadScore }},
  "notes": "{{ $json.message }}"
}
```

### Step 7: Long-term Nurture (Cold Leads)

#### 7.1 Add to Newsletter List
```
Node: HTTP Request (for email marketing platform)
Method: POST
URL: https://api.mailchimp.com/3.0/lists/[LIST_ID]/members
Body:
{
  "email_address": "{{ $json.email }}",
  "status": "subscribed",
  "merge_fields": {
    "FNAME": "{{ $json.name }}",
    "INTENT": "{{ $json.intent }}"
  }
}
```

### Complete Workflow Structure
```
Webhook Trigger
    ↓
Data Validation (IF)
    ↓
AI Lead Analysis (AI Agent + OpenAI)
    ↓
Extract Results (Set)
    ↓
Calculate Score (Function)
    ↓
Route by Priority (Switch)
    ├── Hot Leads
    │   ├── Instant Email
    │   ├── Calendar Reminder
    │   └── Internal Alert
    ├── Warm Leads
    │   ├── Initial Email
    │   ├── Follow-up Series
    │   └── CRM Creation
    └── Cold Leads
        ├── Newsletter Signup
        └── Long-term Nurture
```

---

## Market Intelligence Agent

**Objective:** Automatically monitor market conditions, analyze trends, and provide regular intelligence reports.

### Prerequisites
- Market data APIs (Tavily, Firecrawl)
- Google Sheets for data storage
- Email service for reports

### Step 1: Data Collection Setup

#### 1.1 Schedule Trigger
```
Node: Schedule Trigger
Interval: Daily at 6:00 AM
Timezone: Your local timezone
```

#### 1.2 Define Search Parameters
```
Node: Set
Mode: Manual
Fields:
- searchQueries: [
    "Chicago real estate market trends 2025",
    "median home prices Chicago",
    "Chicago housing inventory levels",
    "mortgage rates forecast"
  ]
- targetLocation: "Chicago, IL"
- dateRange: "last 7 days"
```

### Step 2: Web Search and Data Gathering

#### 2.1 Search Market Data
```
Node: Loop Over Items (for each search query)
    ↓
Node: HTTP Request (Tavily API)
Method: POST
URL: https://api.tavily.com/search
Body:
{
  "api_key": "{{ $credentials.tavilyKey }}",
  "query": "{{ $json.query }}",
  "search_depth": "advanced",
  "include_answer": true,
  "max_results": 5
}
```

#### 2.2 Extract and Clean Data
```
Node: Function
Code:
const results = $input.all();
const marketData = [];

for (const result of results) {
  if (result.json.results) {
    for (const item of result.json.results) {
      marketData.push({
        title: item.title,
        content: item.content,
        url: item.url,
        published_date: item.published_date,
        relevance_score: item.score
      });
    }
  }
}

return marketData.map(item => ({ json: item }));
```

### Step 3: AI Analysis and Insights

#### 3.1 Analyze Market Data with AI
```
Node: AI Agent
System Message: "You are a real estate market analyst. Analyze the provided market data and create insights including:
1. Key market trends
2. Price movements and predictions
3. Inventory levels
4. Notable market events
5. Recommendations for real estate professionals

Format your response as a structured report."
Input: {{ $json.content }}
```

#### 3.2 Connect Analysis Model
```
Node: OpenAI Chat Model
Model: gpt-4o-mini
Temperature: 0.4
Max Tokens: 1500
```

### Step 4: Report Generation

#### 4.1 Structure Market Report
```
Node: Function
Code:
const analysisResults = $input.all();
const currentDate = new Date().toISOString().split('T')[0];

const report = {
  date: currentDate,
  summary: analysisResults[0].json.summary,
  trends: analysisResults[0].json.trends,
  recommendations: analysisResults[0].json.recommendations,
  data_sources: $('Search Market Data').all().map(item => item.json.url)
};

return { json: report };
```

#### 4.2 Save to Google Sheets
```
Node: Google Sheets
Operation: Append Row
Sheet ID: [Your Market Data Sheet ID]
Values:
- {{ $json.date }}
- {{ $json.summary }}
- {{ $json.trends }}
- {{ $json.recommendations }}
```

### Step 5: Report Distribution

#### 5.1 Email Market Report
```
Node: Gmail
To: your-team@company.com
Subject: "Daily Market Intelligence - {{ $now.toFormat('yyyy-MM-dd') }}"
Body: HTML template with formatted report
Attachments: Optional chart or data file
```

#### 5.2 Slack Notification
```
Node: Slack
Channel: #market-intelligence
Message: "📊 Daily Market Report Available
Key Trends: {{ $json.trends }}
Full report: [Link to Google Sheet]"
```

---

## Contract Analysis Agent (RAG)

**Objective:** Use Retrieval Augmented Generation to analyze real estate contracts and documents.

### Prerequisites
- Pinecone vector database
- Document storage (Google Drive)
- OpenAI embeddings

### Step 1: Document Ingestion Setup

#### 1.1 Document Upload Trigger
```
Node: Webhook
Method: POST
Path: /upload-contract
Response Mode: Last Node
```

#### 1.2 Download Document
```
Node: HTTP Request
Method: GET
URL: {{ $json.document_url }}
Response Format: File
```

### Step 2: Document Processing

#### 2.1 Extract Text from Document
```
Node: Function
Code:
// For PDF processing
const fs = require('fs');
const pdf = require('pdf-parse');

const buffer = Buffer.from($input.first().binary.data, 'base64');
const data = await pdf(buffer);

return {
  json: {
    text: data.text,
    pages: data.numpages,
    filename: $json.filename
  }
};
```

#### 2.2 Chunk Text for Vector Storage
```
Node: Function
Code:
const text = $json.text;
const chunkSize = 1000;
const overlap = 200;
const chunks = [];

for (let i = 0; i < text.length; i += (chunkSize - overlap)) {
  const chunk = text.slice(i, i + chunkSize);
  chunks.push({
    text: chunk,
    chunk_id: i / (chunkSize - overlap),
    source_document: $json.filename
  });
}

return chunks.map(chunk => ({ json: chunk }));
```

### Step 3: Vector Storage

#### 3.1 Generate Embeddings
```
Node: OpenAI Embeddings
Model: text-embedding-ada-002
Input: {{ $json.text }}
```

#### 3.2 Store in Pinecone
```
Node: HTTP Request (Pinecone API)
Method: POST
URL: https://[index-name]-[project-id].svc.[environment].pinecone.io/vectors/upsert
Headers:
  Api-Key: {{ $credentials.pineconeKey }}
  Content-Type: application/json
Body:
{
  "vectors": [{
    "id": "{{ $json.source_document }}_{{ $json.chunk_id }}",
    "values": {{ $json.embedding }},
    "metadata": {
      "text": "{{ $json.text }}",
      "document": "{{ $json.source_document }}",
      "chunk": {{ $json.chunk_id }}
    }
  }]
}
```

### Step 4: Contract Analysis Interface

#### 4.1 Analysis Request Trigger
```
Node: Chat Trigger
Welcome Message: "Hi! I can help analyze your real estate contracts. What would you like to know about the document?"
```

#### 4.2 Query Vector Database
```
Node: Function
Code:
// Generate embedding for user query
const query = $json.chatInput;
// This would connect to OpenAI embeddings
```

#### 4.3 Retrieve Relevant Chunks
```
Node: HTTP Request (Pinecone Query)
Method: POST
URL: https://[index-name]-[project-id].svc.[environment].pinecone.io/query
Body:
{
  "vector": {{ $json.queryEmbedding }},
  "topK": 5,
  "includeMetadata": true
}
```

### Step 5: AI-Powered Analysis

#### 5.1 Contract Analysis Agent
```
Node: AI Agent
System Message: "You are a real estate contract specialist. Use the provided contract sections to answer questions about:
- Key terms and conditions
- Potential risks or concerns
- Standard vs. non-standard clauses
- Deadlines and important dates
- Financial obligations

Always cite the specific contract section you're referencing."

Tools: Vector Retrieval, Calculator
```

#### 5.2 Connect Legal Analysis Model
```
Node: OpenAI Chat Model
Model: gpt-4o (for complex legal analysis)
Temperature: 0.2
Max Tokens: 2000
```

### Step 6: Response Formatting

#### 6.1 Structure Analysis Response
```
Node: Function
Code:
const analysis = $json.output;
const sources = $json.sources;

const response = {
  analysis: analysis,
  key_findings: extractKeyFindings(analysis),
  risk_factors: extractRisks(analysis),
  action_items: extractActions(analysis),
  source_sections: sources.map(s => s.metadata.text)
};

return { json: response };
```

---

## Social Media Automation

**Objective:** Automatically create and post property listings across multiple social media platforms.

### Prerequisites
- Social media platform APIs (Facebook, Instagram, LinkedIn)
- Image processing capabilities
- Property database/MLS access

### Step 1: New Listing Trigger

#### 1.1 MLS/Database Webhook
```
Node: Webhook
Method: POST
Path: /new-listing
Expected Data:
{
  "property_id": "123456",
  "address": "123 Main St, Chicago, IL",
  "price": 425000,
  "bedrooms": 3,
  "bathrooms": 2,
  "square_feet": 1800,
  "images": ["url1", "url2", "url3"],
  "description": "Beautiful home...",
  "listing_agent": "John Doe"
}
```

### Step 2: Content Generation

#### 2.1 AI-Generated Social Media Posts
```
Node: AI Agent
System Message: "Create engaging social media posts for a real estate listing. Generate:
1. Facebook post (detailed, 200-300 words)
2. Instagram caption (trendy, with hashtags, 150 words max)
3. LinkedIn post (professional, 200 words)
4. Twitter post (concise, 280 chars max)

Use the property details provided and include compelling selling points."
```

#### 2.2 Process Property Images
```
Node: Function
Code:
const images = $json.images;
const processedImages = [];

for (const imageUrl of images) {
  // Download and process image
  const response = await fetch(imageUrl);
  const buffer = await response.buffer();
  
  processedImages.push({
    url: imageUrl,
    buffer: buffer.toString('base64'),
    filename: `property_${$json.property_id}_${Date.now()}.jpg`
  });
}

return { json: { ....$json, processedImages } };
```

### Step 3: Platform-Specific Posting

#### 3.1 Facebook Page Post
```
Node: HTTP Request (Facebook Graph API)
Method: POST
URL: https://graph.facebook.com/v18.0/[PAGE_ID]/photos
Headers:
  Authorization: Bearer {{ $credentials.facebookToken }}
Body (Form Data):
  message: {{ $json.facebookPost }}
  source: {{ $json.processedImages[0].buffer }}
  published: true
```

#### 3.2 Instagram Post
```
Node: HTTP Request (Instagram Basic Display API)
Method: POST
URL: https://graph.facebook.com/v18.0/[INSTAGRAM_ACCOUNT_ID]/media
Body:
{
  "image_url": "{{ $json.images[0] }}",
  "caption": "{{ $json.instagramCaption }}",
  "access_token": "{{ $credentials.instagramToken }}"
}
```

#### 3.3 LinkedIn Company Page
```
Node: HTTP Request (LinkedIn API)
Method: POST
URL: https://api.linkedin.com/v2/ugcPosts
Headers:
  Authorization: Bearer {{ $credentials.linkedinToken }}
  Content-Type: application/json
Body:
{
  "author": "urn:li:organization:[COMPANY_ID]",
  "lifecycleState": "PUBLISHED",
  "specificContent": {
    "com.linkedin.ugc.ShareContent": {
      "shareCommentary": {
        "text": "{{ $json.linkedinPost }}"
      },
      "shareMediaCategory": "IMAGE",
      "media": [{
        "status": "READY",
        "media": "{{ $json.linkedinImageUrn }}"
      }]
    }
  }
}
```

### Step 4: Performance Tracking

#### 4.1 Log Social Media Posts
```
Node: Google Sheets
Operation: Append Row
Sheet: Social Media Tracking
Values:
- {{ $now.toISO() }}
- {{ $json.property_id }}
- {{ $json.address }}
- Facebook,Instagram,LinkedIn
- {{ $json.facebookPostId }}
- {{ $json.instagramPostId }}
- {{ $json.linkedinPostId }}
```

#### 4.2 Schedule Follow-up Analysis
```
Node: Wait (24 hours)
    ↓
Node: HTTP Request (Get engagement metrics)
    ↓
Node: Google Sheets (Update with metrics)
```

---

## Multi-Agent System Architecture

**Objective:** Create a coordinated system of multiple AI agents working together for complex real estate operations.

### System Overview
```
Lead Management Agent
    ↓
Property Research Agent
    ↓
Communication Agent
    ↓
Transaction Coordination Agent
```

### Step 1: Central Coordination Hub

#### 1.1 Master Workflow Trigger
```
Node: Webhook
Path: /new-client-request
Method: POST
Response Mode: Wait for Sub-workflow
```

#### 1.2 Request Classification
```
Node: AI Agent (Coordinator)
System Message: "Analyze the client request and determine which agents should handle it:
1. Lead Management - for new inquiries
2. Property Research - for property searches
3. Communication - for ongoing client communication
4. Transaction - for active deals

Respond with a JSON array of required agents and their tasks."
```

### Step 2: Agent Orchestration

#### 2.1 Execute Workflow Trigger (for each agent)
```
Node: Loop Over Items
    ↓
Node: Execute Workflow
Workflow: {{ $json.agentType }}
Wait for completion: true
Data to send: {{ $json.taskDetails }}
```

#### 2.2 Collect Agent Results
```
Node: Merge (wait for all agents)
    ↓
Node: AI Agent (Result Synthesizer)
System Message: "Combine the results from multiple agents into a comprehensive response for the client."
```

### Step 3: Individual Agent Workflows

#### Lead Management Agent
```
Webhook Trigger (/lead-agent)
    ↓
Qualification Logic
    ↓
CRM Integration
    ↓
Initial Communication
    ↓
Return Results
```

#### Property Research Agent
```
Webhook Trigger (/research-agent)
    ↓
MLS Search
    ↓
Market Analysis
    ↓
Property Recommendations
    ↓
Return Results
```

#### Communication Agent
```
Webhook Trigger (/communication-agent)
    ↓
Message Classification
    ↓
Response Generation
    ↓
Multi-channel Delivery
    ↓
Return Results
```

---

## Testing and Debugging Guidelines

### Testing Strategy

#### 1. Unit Testing (Individual Nodes)
- Test each node with sample data
- Verify data transformations
- Check error handling

#### 2. Integration Testing (Workflow Sections)
- Test data flow between connected nodes
- Verify conditional logic
- Test API integrations

#### 3. End-to-End Testing (Complete Workflows)
- Test with realistic data scenarios
- Verify final outputs
- Test error recovery

### Debugging Techniques

#### 1. Use n8n's Built-in Tools
- **Node Execution Data:** Check input/output for each node
- **Expression Editor:** Test expressions before using them
- **Workflow History:** Review past executions

#### 2. Add Debug Nodes
```
Node: Set (Debug Info)
Fields:
- debug_step: "Lead qualification"
- debug_data: {{ JSON.stringify($json) }}
- debug_timestamp: {{ $now.toISO() }}
```

#### 3. Error Handling Patterns
```
Node: Try-Catch Pattern
Try Path: Main workflow logic
Catch Path: Error Trigger → Notification → Fallback action
```

### Common Issues and Solutions

#### Data Type Mismatches
**Problem:** String vs. Number comparisons
**Solution:** Use explicit type conversion
```javascript
// Convert to number
{{ Number($json.price) > 500000 }}

// Convert to string
{{ String($json.bedrooms) === "3" }}
```

#### API Rate Limits
**Problem:** Too many API calls
**Solution:** Add delays and batch processing
```
Node: Wait
Time: 1 second
↓
Node: HTTP Request
```

#### Memory Issues with Large Datasets
**Problem:** Workflow timeouts
**Solution:** Use pagination and chunking
```
Node: Split in Batches
Batch Size: 10
```

---

## Performance Optimization

### Workflow Optimization

#### 1. Minimize HTTP Requests
- Batch API calls when possible
- Cache frequently accessed data
- Use webhooks instead of polling

#### 2. Efficient Data Processing
```javascript
// Good: Process in bulk
const processedItems = items.map(item => processItem(item));

// Bad: Process one by one with separate nodes
```

#### 3. Smart Error Handling
- Fail fast for invalid data
- Implement circuit breaker patterns
- Use exponential backoff for retries

### Resource Management

#### 1. Memory Usage
- Process large files in chunks
- Clear unnecessary data between steps
- Use streaming for file operations

#### 2. Execution Time
- Set appropriate timeouts
- Use asynchronous operations
- Implement parallel processing where possible

### Monitoring and Maintenance

#### 1. Performance Metrics
- Track execution times
- Monitor error rates
- Measure throughput

#### 2. Regular Maintenance
- Update API credentials
- Review and optimize workflows
- Archive old execution data

---

*This guide provides the foundation for building robust, scalable real estate automation workflows. Each section can be adapted to your specific business needs and technical requirements.* 