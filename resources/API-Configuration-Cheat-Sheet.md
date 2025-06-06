# API Configuration Cheat Sheet

**n8n AI Agent Training Course - Real Estate Automation**  
*Quick Reference for Setting Up Required API Keys and Services*

---

## Table of Contents

1. [OpenRouter Configuration](#openrouter-configuration)
2. [Pinecone Vector Database Setup](#pinecone-vector-database-setup)
3. [Google Cloud Console APIs](#google-cloud-console-apis)
4. [Tavily Search API](#tavily-search-api)
5. [Perplexity AI API](#perplexity-ai-api)
6. [Firecrawl API](#firecrawl-api)
7. [Telegram Bot Setup](#telegram-bot-setup)
8. [n8n Credential Configuration](#n8n-credential-configuration)
9. [Troubleshooting Common Issues](#troubleshooting-common-issues)

---

## OpenRouter Configuration

**Purpose:** Access to multiple AI models through a single API  
**Use Case:** LLM integration for AI agents and chat models

### Account Setup
1. **Sign Up:** Go to [openrouter.ai](https://openrouter.ai/)
2. **Email Verification:** Check your email and verify account
3. **Account Dashboard:** Navigate to your dashboard after login

### API Key Generation
1. **Navigate to API Keys:** Click on "API Keys" in the left sidebar
2. **Create New Key:** Click "Create API Key"
3. **Name Your Key:** Use descriptive name like "n8n-real-estate-course"
4. **Copy Key:** Save the key immediately (starts with `sk-or-v1-`)

### n8n Configuration
```
Credential Type: OpenRouter
API Key: sk-or-v1-[your-key-here]
Base URL: https://openrouter.ai/api/v1
```

### Available Models for Real Estate
- **GPT-4o Mini:** `openai/gpt-4o-mini` (Cost-effective, fast)
- **Claude 3.5 Sonnet:** `anthropic/claude-3.5-sonnet` (Excellent reasoning)
- **Llama 3.1 8B:** `meta-llama/llama-3.1-8b-instruct:free` (Free option)

### Cost Considerations
- **GPT-4o Mini:** ~$0.15/1M input tokens, $0.60/1M output tokens
- **Claude 3.5 Sonnet:** ~$3.00/1M input tokens, $15.00/1M output tokens
- **Free Models:** Rate limited but good for testing

---

## Pinecone Vector Database Setup

**Purpose:** Vector storage for RAG (Retrieval Augmented Generation)  
**Use Case:** Contract analysis, property information retrieval

### Account Creation
1. **Sign Up:** Visit [pinecone.io](https://www.pinecone.io/)
2. **Choose Plan:** Start with free tier (includes 1 project, 1 index)
3. **Email Verification:** Complete email verification process

### API Key Generation
1. **Access Console:** Log into Pinecone console
2. **Navigate to API Keys:** Click "API Keys" in left sidebar
3. **Create Key:** Click "Create API Key"
4. **Save Credentials:** Copy both API key and environment name

### n8n Configuration
```
Credential Type: Pinecone
API Key: [your-pinecone-api-key]
Environment: [your-environment] (e.g., us-east1-gcp)
```

### Index Configuration for Real Estate
```json
{
  "name": "real-estate-docs",
  "dimension": 1536,
  "metric": "cosine",
  "pods": 1,
  "replicas": 1,
  "pod_type": "p1.x1"
}
```

### Best Practices
- **Naming Convention:** Use descriptive index names
- **Dimensions:** 1536 for OpenAI embeddings, 768 for sentence-transformers
- **Metric:** Cosine similarity for text embeddings

---

## Google Cloud Console APIs

**Purpose:** Access to Gmail, Drive, Sheets, Docs, Calendar, Tasks  
**Use Case:** Document management, email automation, calendar integration

### Initial Setup
1. **Google Cloud Console:** Go to [console.cloud.google.com](https://console.cloud.google.com/)
2. **Create Project:** Click "New Project" and name it "n8n-real-estate"
3. **Enable Billing:** Required for API access (some have free quotas)

### Required APIs to Enable
Navigate to "APIs & Services" → "Library" and enable:

#### Core APIs
- **Gmail API:** Email automation and management
- **Google Drive API:** Document storage and retrieval
- **Google Sheets API:** Spreadsheet integration
- **Google Docs API:** Document creation and editing
- **Google Calendar API:** Appointment scheduling
- **Google Tasks API:** Task management

#### Enable Each API
1. Search for API name in the library
2. Click on the API
3. Click "Enable"
4. Wait for activation (usually 1-2 minutes)

### Service Account Creation
1. **Navigate:** APIs & Services → Credentials
2. **Create Credentials:** Click "Create Credentials" → "Service Account"
3. **Account Details:**
   - Name: `n8n-real-estate-service`
   - Description: `Service account for n8n real estate automation`
4. **Download JSON:** After creation, download the JSON key file

### OAuth 2.0 Setup (Alternative)
1. **Create OAuth Client:** APIs & Services → Credentials → Create Credentials → OAuth client ID
2. **Application Type:** Web application
3. **Authorized Redirect URIs:** Add your n8n webhook URL
4. **Download Credentials:** Save client ID and secret

### n8n Configuration Options

#### Option 1: Service Account (Recommended)
```
Credential Type: Google Service Account
Email: [service-account-email]
Private Key: [paste-private-key-from-json]
```

#### Option 2: OAuth2
```
Credential Type: Google OAuth2
Client ID: [your-client-id]
Client Secret: [your-client-secret]
```

### Scope Requirements
Ensure your credentials have these scopes:
- `https://www.googleapis.com/auth/gmail.modify`
- `https://www.googleapis.com/auth/drive`
- `https://www.googleapis.com/auth/spreadsheets`
- `https://www.googleapis.com/auth/documents`
- `https://www.googleapis.com/auth/calendar`
- `https://www.googleapis.com/auth/tasks`

---

## Tavily Search API

**Purpose:** Real-time web search for AI agents  
**Use Case:** Market research, property information gathering

### Account Setup
1. **Visit Tavily:** Go to [tavily.com](https://tavily.com/)
2. **Sign Up:** Create account with email
3. **Email Verification:** Check email and verify

### API Key Generation
1. **Dashboard Access:** Log into your Tavily dashboard
2. **API Section:** Navigate to "API Keys" or "Developers" section
3. **Generate Key:** Click "Generate API Key"
4. **Copy Key:** Save the API key securely

### n8n Configuration
```
Credential Type: HTTP Request Node
Header Name: Authorization
Header Value: Bearer [your-tavily-api-key]
```

### Usage Limits
- **Free Tier:** 1,000 searches per month
- **Rate Limits:** 100 requests per minute
- **Upgrade Options:** Available for higher usage

### Search Parameters
```json
{
  "api_key": "your-api-key",
  "query": "real estate market trends Chicago 2025",
  "search_depth": "advanced",
  "include_answer": true,
  "include_raw_content": false,
  "max_results": 10
}
```

---

## Perplexity AI API

**Purpose:** AI-powered search and analysis  
**Use Case:** Market analysis, research summaries

### Account Setup
1. **Perplexity Console:** Visit [perplexity.ai](https://www.perplexity.ai/)
2. **Sign Up:** Create account
3. **Pro Subscription:** May be required for API access

### API Access
1. **Settings:** Navigate to account settings
2. **API Section:** Look for "API" or "Developer" options
3. **Generate Key:** Create new API key
4. **Documentation:** Review API documentation for endpoints

### n8n Configuration
```
Credential Type: HTTP Request Node
URL: https://api.perplexity.ai/chat/completions
Headers:
  Authorization: Bearer [your-api-key]
  Content-Type: application/json
```

### Sample Request Format
```json
{
  "model": "llama-3.1-sonar-small-128k-online",
  "messages": [
    {
      "role": "system",
      "content": "You are a real estate market analyst."
    },
    {
      "role": "user",
      "content": "Analyze current market trends in Chicago real estate"
    }
  ]
}
```

---

## Firecrawl API

**Purpose:** Web scraping and content extraction  
**Use Case:** Property listing scraping, market data collection

### Account Setup
1. **Firecrawl Website:** Go to [firecrawl.dev](https://www.firecrawl.dev/)
2. **Sign Up:** Create account with email
3. **Plan Selection:** Choose appropriate plan (free tier available)

### API Key Generation
1. **Dashboard:** Access your Firecrawl dashboard
2. **API Keys:** Navigate to API keys section
3. **Create Key:** Generate new API key
4. **Save Key:** Store securely for n8n configuration

### n8n Configuration
```
Credential Type: HTTP Request Node
URL: https://api.firecrawl.dev/v0/
Headers:
  Authorization: Bearer [your-firecrawl-api-key]
  Content-Type: application/json
```

### Common Endpoints
- **Scrape:** `/scrape` - Extract content from single URL
- **Crawl:** `/crawl` - Crawl entire website
- **Search:** `/search` - Search web and extract results

### Usage Examples
```json
{
  "url": "https://example-real-estate-site.com",
  "formats": ["markdown", "html"],
  "onlyMainContent": true,
  "includeTags": ["title", "meta", "price"]
}
```

---

## Telegram Bot Setup

**Purpose:** Notifications and communication interface  
**Use Case:** Instant notifications, client communication

### Bot Creation
1. **Start BotFather:** Open Telegram and search for @BotFather
2. **Create Bot:** Send `/newbot` command
3. **Bot Name:** Choose a name (e.g., "Real Estate Assistant")
4. **Username:** Choose username (must end with 'bot')
5. **Save Token:** Copy the bot token provided

### Bot Configuration
1. **Bot Settings:** Use BotFather to configure:
   - Description
   - About text
   - Profile picture
   - Commands menu

### n8n Configuration
```
Credential Type: Telegram
Bot Token: [your-bot-token]
```

### Useful Bot Commands Setup
Send these to BotFather using `/setcommands`:
```
start - Start the bot
help - Get help information
status - Check system status
notify - Toggle notifications
```

### Getting Chat ID
1. **Add Bot:** Add your bot to a chat or group
2. **Send Message:** Send any message to the bot
3. **Get Updates:** Visit `https://api.telegram.org/bot[BOT_TOKEN]/getUpdates`
4. **Find ID:** Look for "chat":{"id": in the response

---

## n8n Credential Configuration

### General Best Practices
1. **Descriptive Names:** Use clear, descriptive credential names
2. **Test Credentials:** Always test after setup
3. **Environment Separation:** Use different credentials for development/production
4. **Regular Rotation:** Update API keys periodically

### Common Credential Types

#### HTTP Request Authentication
```
Type: Predefined Credential Type
Credential: HTTP Request Node
Authentication: Bearer Token
Token: [your-api-key]
```

#### OAuth2 Services
```
Type: OAuth2 API
Client ID: [your-client-id]
Client Secret: [your-client-secret]
Authorization URL: [service-specific]
Access Token URL: [service-specific]
Scope: [required-scopes]
```

### Credential Testing
1. **Test Button:** Use the "Test" button in credential setup
2. **Simple Workflow:** Create test workflow to verify connectivity
3. **Error Handling:** Check error messages for configuration issues

### Security Considerations
1. **Least Privilege:** Only grant necessary permissions
2. **Regular Audits:** Review and remove unused credentials
3. **Secure Storage:** n8n encrypts credentials automatically
4. **Access Control:** Limit who can view/edit credentials

---

## Troubleshooting Common Issues

### OpenRouter Issues
**Problem:** `Invalid API key` error  
**Solution:** 
- Verify key starts with `sk-or-v1-`
- Check for extra spaces or characters
- Regenerate key if needed

**Problem:** Model not available  
**Solution:**
- Check model name spelling
- Verify model is active on OpenRouter
- Try alternative model

### Google APIs Issues
**Problem:** `403 Forbidden` error  
**Solution:**
- Ensure APIs are enabled in Google Cloud Console
- Check service account permissions
- Verify billing is enabled

**Problem:** OAuth2 authorization fails  
**Solution:**
- Check redirect URI configuration
- Verify OAuth consent screen setup
- Ensure proper scopes are requested

### Pinecone Issues
**Problem:** `Index not found` error  
**Solution:**
- Verify index name spelling
- Check if index exists in dashboard
- Ensure proper environment configuration

**Problem:** Dimension mismatch  
**Solution:**
- Verify embedding model dimensions
- Check index configuration
- Recreate index with correct dimensions

### General API Issues
**Problem:** Rate limiting errors  
**Solution:**
- Implement exponential backoff
- Check rate limits in API documentation
- Consider upgrading plan

**Problem:** Timeout errors  
**Solution:**
- Increase timeout settings in n8n
- Check API endpoint status
- Implement retry logic

### Network Issues
**Problem:** Connection timeouts  
**Solution:**
- Check internet connectivity
- Verify firewall settings
- Test with different endpoints

**Problem:** SSL certificate errors  
**Solution:**
- Check system time and date
- Update certificates if self-hosted
- Verify API endpoint URLs

### Authentication Debugging Steps
1. **Check API Key Format:** Ensure correct prefix/format
2. **Test with Curl:** Use command line to test API
3. **Review Documentation:** Check for recent API changes
4. **Check Quotas:** Verify usage limits not exceeded
5. **Contact Support:** Reach out to API provider if needed

### Common Error Codes
- **400 Bad Request:** Check request format and required parameters
- **401 Unauthorized:** Verify API key and authentication
- **403 Forbidden:** Check permissions and enabled features
- **404 Not Found:** Verify endpoint URL and resource existence
- **429 Too Many Requests:** Implement rate limiting and retry logic
- **500 Internal Server Error:** Check API status page and retry

---

## Quick Reference Summary

| Service | Purpose | Key Format | Monthly Limits |
|---------|---------|------------|----------------|
| OpenRouter | AI Models | `sk-or-v1-...` | Pay per use |
| Pinecone | Vector DB | Standard key | 1 free index |
| Google Cloud | Productivity APIs | JSON/OAuth2 | Varies by API |
| Tavily | Web Search | Standard key | 1,000 free searches |
| Perplexity | AI Search | `Bearer ...` | Subscription based |
| Firecrawl | Web Scraping | Standard key | 500 free credits |
| Telegram | Messaging | Bot token | Unlimited |

---

*Keep this cheat sheet handy during the workshop for quick reference. All credentials should be configured before the course begins to maximize hands-on learning time.* 