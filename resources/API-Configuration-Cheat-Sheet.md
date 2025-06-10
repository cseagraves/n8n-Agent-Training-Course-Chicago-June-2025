# API Configuration Cheat Sheet

**n8n AI Agent Training Course - June 14th, 2025**  
**Quick Reference Guide for Service Setup**

---

## Table of Contents

1. [OpenRouter (AI Models)](#openrouter-ai-models)
2. [Pinecone (Vector Database)](#pinecone-vector-database)
3. [Google Cloud APIs](#google-cloud-apis)
4. [Tavily Search](#tavily-search)
5. [Perplexity AI](#perplexity-ai)
6. [Firecrawl](#firecrawl)
7. [BatchData](#batchdata)
8. [Telegram Bot](#telegram-bot)
9. [n8n Credential Setup](#n8n-credential-setup)
10. [Troubleshooting](#troubleshooting)

---

## OpenRouter (AI Models)

**Purpose:** Access to multiple AI models (GPT-4, Claude, Gemini, etc.) through a single API  
**Cost:** Pay-per-use, typically $0.002-0.06 per 1K tokens  
**Website:** https://openrouter.ai/  
**n8n Integration:** ✅ **Works with OpenAI credential** (supports compatible base URL)

### Setup Steps:

1. **Create Account**
   - Go to https://openrouter.ai/
   - Click "Sign Up" and create account
   - Verify email address

2. **Get API Key**
   - Navigate to "Keys" section in dashboard
   - Click "Create Key"
   - Name it "n8n-training" 
   - Copy the API key (starts with `sk-or-v1-...`)

3. **Add Credits**
   - Go to "Credits" section
   - Add $10-20 for training (should last months)
   - Choose payment method

### n8n Configuration:
```
Credential Type: OpenAI
API Key: [Your OpenRouter API key]
Base URL: https://openrouter.ai/api/v1
```

### Recommended Models:
- **GPT-4o Mini:** `openai/gpt-4o-mini` (fast, cheap)
- **Claude 3.5 Sonnet:** `anthropic/claude-3.5-sonnet` (high quality)
- **GPT-4o:** `openai/gpt-4o` (balanced performance)

---

## Pinecone (Vector Database)

**Purpose:** Store and search document embeddings for RAG (Retrieval Augmented Generation)  
**Cost:** Free tier: 1 index, 100K vectors  
**Website:** https://www.pinecone.io/  
**n8n Integration:** ✅ **Official Vector Store nodes** (AI/LangChain integration)

### Setup Steps:

1. **Create Account**
   - Go to https://www.pinecone.io/
   - Click "Sign Up Free"
   - Complete registration

2. **Create Index**
   - In dashboard, click "Create Index"
   - Name: `real-estate-docs`
   - Dimensions: `1536` (for OpenAI embeddings)
   - Metric: `cosine`
   - Environment: Choose closest region

3. **Get API Key**
   - Go to "API Keys" section
   - Copy the API key
   - Note your environment (e.g., `us-east-1-aws`)

### n8n Configuration:
```
Credential Type: Pinecone
API Key: [Your Pinecone API key]
Environment: [Your environment, e.g., us-east-1-aws]
```

### Available as Vector Store:
- **Vector Store Insert Node:** Add documents and embeddings to Pinecone index
- **Vector Store Retriever Node:** Search for similar documents using vector similarity
- **Memory Vector Store:** Use Pinecone as memory storage for AI agents
- **LangChain Integration:** Works seamlessly with n8n's AI agent workflows

---

## Google Cloud APIs

**Purpose:** Access Gmail, Drive, Sheets, Docs, Calendar, Tasks  
**Cost:** Free tier covers most training needs  
**Website:** https://console.cloud.google.com/  
**n8n Integration:** ✅ **Official Google Service Account credential** (supports all Google services)

### Setup Steps:

1. **Create Project**
   - Go to Google Cloud Console
   - Click "New Project"
   - Name: "n8n-real-estate-training"
   - Create project

2. **Enable APIs**
   Navigate to "APIs & Services" > "Library" and enable:
   - Gmail API
   - Google Drive API
   - Google Sheets API
   - Google Docs API
   - Google Calendar API
   - Google Tasks API

3. **Create Service Account**
   - Go to "APIs & Services" > "Credentials"
   - Click "Create Credentials" > "Service Account"
   - Name: "n8n-service-account"
   - Create and continue

4. **Generate Key**
   - Click on created service account
   - Go to "Keys" tab
   - Click "Add Key" > "Create New Key"
   - Choose JSON format
   - Download the JSON file

5. **Share Resources** (Important!)
   - Share Google Sheets/Docs with service account email
   - Email format: `service-account-name@project-id.iam.gserviceaccount.com`
   - Give "Editor" permissions

### n8n Configuration:
```
Credential Type: Google Service Account
Service Account Email: [From JSON file]
Private Key: [From JSON file - the entire key including headers]
```

---

## Tavily Search

**Purpose:** Real-time web search for AI agents  
**Cost:** Free tier: 1,000 searches/month  
**Website:** https://tavily.com/  
**n8n Integration:** ❌ **Requires HTTP Request node** (no official n8n node)

### Setup Steps:

1. **Create Account**
   - Go to https://tavily.com/
   - Click "Get Started"
   - Sign up with email

2. **Get API Key**
   - Complete onboarding
   - Go to dashboard
   - Copy API key from main page

### n8n Configuration:
```
Credential Type: HTTP Request (Generic)
Authentication: None
Headers: 
  - Name: Authorization
  - Value: Bearer [Your Tavily API key]
```

### Usage Example:
```
POST https://api.tavily.com/search
Body: {
  "api_key": "your-api-key",
  "query": "real estate market trends 2025",
  "search_depth": "basic"
}
```

---

## Perplexity AI

**Purpose:** AI-powered search and research  
**Cost:** Free tier: 5 searches/day, Pro: $20/month  
**Website:** https://www.perplexity.ai/  
**n8n Integration:** ❌ **Requires HTTP Request node** (no official n8n node)

### Setup Steps:

1. **Create Account**
   - Go to https://www.perplexity.ai/
   - Sign up with email or Google

2. **Get API Key**
   - Go to https://www.perplexity.ai/settings/api
   - Click "Generate" to create API key
   - Copy the key

### n8n Configuration:
```
Credential Type: HTTP Request (Generic)
Authentication: None
Headers:
  - Name: Authorization
  - Value: Bearer [Your Perplexity API key]
  - Name: Content-Type
  - Value: application/json
```

---

## Firecrawl

**Purpose:** Web scraping and content extraction  
**Cost:** Free tier: 500 pages/month, Hobby: $16/month (3,000 credits), Standard: $83/month (100,000 credits)  
**Website:** https://www.firecrawl.dev/  
**n8n Integration:** ❌ **Requires HTTP Request node** (no official n8n node)

### Setup Steps:

1. **Create Account**
   - Go to https://www.firecrawl.dev/
   - Click "Get Started"
   - Sign up with GitHub or email

2. **Get API Key**
   - Go to dashboard
   - Navigate to "API Keys"
   - Copy the API key (starts with `fc-`)

### n8n Configuration:
```
Credential Type: HTTP Request (Generic)
Authentication: None
Headers:
  - Name: Authorization
  - Value: Bearer [Your Firecrawl API key]
  - Name: Content-Type
  - Value: application/json
```

### Usage Details:
- **Base URL:** https://api.firecrawl.dev
- **Credit Usage:** 1 credit per page for scrape/crawl, 1 credit per call for map, 1 credit per result for search
- **Features:** Handles JavaScript, dynamic content, authentication flows, returns markdown/JSON/HTML

---

## BatchData

**Purpose:** Property data, owner information, and lead generation for real estate  
**Cost:** Pay-as-you-go pricing (no subscription required)  
**Website:** https://batchdata.com/  
**n8n Integration:** ❌ **Requires HTTP Request node** (no official n8n node)

### Setup Steps:

1. **Create Account**
   - Go to https://batchdata.com/
   - Click "Talk to an Expert" or sign up
   - Complete registration

2. **Get API Key**
   - Access your dashboard at https://app.batchdata.com/
   - Navigate to API settings
   - Generate and copy your API key

3. **Review Documentation**
   - Visit https://developer.batchdata.com/
   - Review API endpoints and data dictionary
   - Plan your data requirements

### n8n Configuration:
```
Credential Type: HTTP Request (Generic)
Authentication: None
Headers:
  - Name: Authorization
  - Value: Bearer [Your BatchData API key]
  - Name: Content-Type
  - Value: application/json
```

### Available Data Services:
- **Property Enrichment:** 300+ property data points including ownership history, characteristics, mortgage details
- **Contact Enrichment + Skip Tracing:** Accurate contact details and phone/email verification
- **Property Search API:** Advanced filtering and nested queries for lead generation
- **Phone Number Verification:** Validate phone numbers for compliance and accuracy
- **CRM Enrichment:** Automatic data synchronization with existing CRM systems

### Usage Examples:
- Lead generation based on property criteria (foreclosure status, equity, location)
- Property verification and due diligence
- Contact information discovery and validation
- Market analysis and investment opportunity identification

---

## Telegram Bot

**Purpose:** Create chatbots and notifications  
**Cost:** Free  
**Setup:** Through Telegram app  
**n8n Integration:** ✅ **Official Telegram node** (comprehensive operations support)

### Setup Steps:

1. **Create Bot**
   - Open Telegram app
   - Search for "@BotFather"
   - Send `/newbot` command
   - Follow prompts to name your bot
   - Save the bot token

2. **Get Chat ID** (for notifications)
   - Send a message to your bot
   - Visit: `https://api.telegram.org/bot[BOT_TOKEN]/getUpdates`
   - Find your chat ID in the response

### n8n Configuration:
```
Credential Type: Telegram
Access Token: [Your bot token from BotFather]
```

### Available Operations:
- **Chat Operations:** Manage chat members, settings, and permissions
- **Message Operations:** Send/receive text, media, and files
- **Callback Operations:** Handle inline keyboard interactions
- **File Operations:** Upload and download files

---

## n8n Credential Setup

### Adding Credentials to n8n:

1. **Navigate to Credentials**
   - In n8n, click "Credentials" in left sidebar
   - Click "Add Credential"

2. **Select Credential Type**
   - Choose appropriate type from list
   - Fill in required fields
   - Test connection if available

3. **Name Your Credentials**
   - Use descriptive names like:
     - "OpenRouter-Training"
     - "Google-Service-Account"
     - "Pinecone-RealEstate"

### Security Best Practices:

- ✅ **Never share API keys in screenshots or public forums**
- ✅ **Use separate credentials for training vs. production**
- ✅ **Regularly rotate API keys**
- ✅ **Monitor usage and costs**
- ✅ **Set up billing alerts**

---

## Troubleshooting

### Common Issues and Solutions:

#### OpenRouter Issues:
**Problem:** "Invalid API key" error  
**Solution:** 
- Ensure you're using the full key including `sk-or-v1-` prefix
- Check that base URL is set to `https://openrouter.ai/api/v1`
- Verify you have credits in your account

#### Google APIs Issues:
**Problem:** "Access denied" or "File not found"  
**Solution:**
- Share Google Sheets/Docs with service account email
- Ensure all required APIs are enabled
- Check service account has correct permissions

#### Pinecone Issues:
**Problem:** "Index not found"  
**Solution:**
- Verify index name matches exactly (case-sensitive)
- Check environment setting matches your Pinecone dashboard
- Ensure index is in "Ready" state

#### General API Issues:
**Problem:** Rate limiting or quota exceeded  
**Solution:**
- Check your usage in service dashboards
- Upgrade to paid tiers if needed
- Implement delays between requests
- Use error handling in workflows

### Testing Your Setup:

1. **Create Test Workflow**
   - Add Manual Trigger
   - Add HTTP Request node
   - Test each API connection

2. **Verify Credentials**
   - Use "Test" button in credential setup
   - Check for successful responses
   - Monitor error logs

3. **Check Permissions**
   - Ensure all required scopes are granted
   - Verify service account permissions
   - Test with minimal data first

### Getting Help:

- **n8n Community:** https://community.n8n.io/
- **Documentation:** https://docs.n8n.io/
- **Course Support:** cayman@caiyman.ai

---

## Quick Reference Summary

| Service | Purpose | Free Tier | API Key Location | n8n Integration |
|---------|---------|-----------|------------------|-----------------|
| OpenRouter | AI Models | Pay-per-use | Dashboard > Keys | ✅ |
| Pinecone | Vector DB | 100K vectors | Dashboard > API Keys | ✅ |
| Google Cloud | Workspace APIs | Generous limits | Service Account JSON | ✅ |
| Tavily | Web Search | 1K searches/month | Dashboard | ❌ |
| Perplexity | AI Search | 5 searches/day | Settings > API | ❌ |
| Firecrawl | Web Scraping | 500 pages/month | Dashboard > API Keys | ❌ |
| BatchData | Property Data | Pay-as-you-go | API Settings | ❌ |
| Telegram | Bot Platform | Free | @BotFather | ✅ |

### Essential URLs:
- **OpenRouter:** https://openrouter.ai/keys
- **Pinecone:** https://app.pinecone.io/
- **Google Cloud:** https://console.cloud.google.com/
- **Tavily:** https://app.tavily.com/
- **Perplexity:** https://www.perplexity.ai/settings/api
- **Firecrawl:** https://www.firecrawl.dev/app
- **BatchData:** https://app.batchdata.com/

---

*Keep this cheat sheet handy during the workshop! We'll be using all these services to build powerful AI workflows for real estate automation.* 