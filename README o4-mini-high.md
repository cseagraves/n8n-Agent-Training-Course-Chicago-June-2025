# Chicago 1-Day n8n AI Agent Training Course (Real Estate Focus)

Check out my website [Caiyman AI](https://www.caiyman.ai/) for more info about me and what I do.

**Date:** June 14, 2025  
**Time:** 9:00 AM – 12:00 PM / 2:00 PM – 4:00 PM

---

## 1. Attendees & Course Context
- **Participants:** Experienced real estate agents and real estate professionals.  
- **AI Experience:** Mix of beginners and those familiar with AI agents; assume most have some exposure.  
- **Platform:** Training conducted using [n8n](https://n8n.io/) on the n8n cloud.  
- **Approach:** PowerPoint background slides and hands-on building real estate–themed agent templates.  
- **Resource Links:**  
  - [n8n Blog](https://blog.n8n.io/)  
  - [n8n LLM Agents](https://blog.n8n.io/llm-agents/)  
  - [How to Build AI Agent](https://blog.n8n.io/how-to-build-ai-agent/)  
  - [RAG Chatbot Tutorial](https://blog.n8n.io/rag-chatbot/)  
  - [n8n Community Forum](https://community.n8n.io/)  
- **Real Estate Focus:** Templates centered on real estate use cases, including personal assistant–style workflows.

---

## 2. Pre-Course Preparations
- Create an [n8n Cloud](https://n8n.io/) account and add the instructor (e.g., cayman@collectiveai.ai) as a user.  
- Obtain API keys for:  
  - [OpenRouter/OpenAI](https://openai.com/)  
  - [Pinecone](https://www.pinecone.io/) (or equivalent vector store)  
  - [Google Drive](https://drive.google.com/), Gmail, [Google Sheets](https://sheets.google.com/), [Google Docs](https://docs.google.com/)  
  - [Tavily](https://www.tavily.com/), [Perplexity](https://www.perplexity.ai/), or other research APIs  
- Complete credentials setup at least 2–3 days before training to avoid falling behind.

---

## 3. Course Overview & Learning Objectives
- Attendees learn to build AI agents in n8n focusing on real estate applications.  
- **Core Concepts:** Prompting, chaining LLMs, agent basics, multi-agent architectures, API integrations, webhooks, self-hosting.  
- **Emphasis on hands-on building** with real estate–themed templates.

---

## 4. Workflow Examples (Overview)
- Personal Assistant Agent  
- Lead Generator / CRM Integration  
- RAG Contract Agent  
- Redfin Market Analysis / Listing Search  
- Social Media Agents  
- Image / Video Agents  
- MCP on n8n Cloud: Connecting multiple AI models.

---

## 5. Course Schedule (Outline with Simplified Details)

**09:00 AM – 09:10 AM: Introduction & Housekeeping**  
- Welcome, agenda review, confirm n8n access and credentials.

**09:10 AM – 09:40 AM: AI & Agents Foundations**  
- Define AI/LLMs, differences between chatbots and autonomous agents.

**09:40 AM – 10:15 AM: n8n Foundations & LLM Connectivity**  
- Demonstrate n8n workflows, LLM node setup, and basic debugging.

**10:15 AM – 10:20 AM: Break**  
- Quick 5-minute break.

**10:20 AM – 11:15 AM: Hands-On Session #1 – Personal Assistant Agent**  
- Build real estate Q&A agent with webhook, LLM, Google Sheets logging, and Gmail.

**11:15 AM – 12:00 PM: Hands-On Session #2 – Lead Generator & CRM Integration**  
- Create lead generation workflow with scheduled fetch, filtering, and CRM logging.

**12:00 PM – 02:00 PM: Lunch Break**  
- Networking, optional troubleshooting; instructor available via Slack/email.

**02:00 PM – 02:05 PM: Afternoon Welcome & Recap**  
- Recap morning sessions and address quick questions.

**02:05 PM – 02:40 PM: Hands-On Session #3 – RAG Contract Agent**  
- Build contract analysis RAG agent using Pinecone embeddings and LLM.

**02:40 PM – 03:05 PM: Hands-On Session #4 – Market Analysis & Listing Search Agent**  
- Build market analysis workflow with CSV-to-JSON conversion, metric calculations, and reporting.

**03:05 PM – 03:25 PM: Hands-On Session #5 – Social Media & Content Agent**  
- Create social media content agent generating posts from recent listings.

**03:25 PM – 03:45 PM: Advanced Topics & Multi-Agent Architectures**  
- Overview of multi-agent patterns, error handling, and MCP setup.

**03:45 PM – 04:00 PM: Q&A, Next Steps, & Wrap-Up**  
- Summarize key learnings, share templates, resources, and office hours details.

---

## 6. Recommended Slide Deck Structure (with Added Details)
- **Title Slide:**  
  - Course name, date, and instructor contact (e.g., Cayman Seagraves).  
- **Agenda Overview:**  
  - Morning: Foundations, Personal Assistant, Lead Generator  
  - Afternoon: RAG, Market Analysis, Social Media, Advanced Topics.  
- **AI & Agent Fundamentals:**  
  - Explain LLMs (GPT-4, [Claude](https://claude.ai/)) and their use in real estate (market trends, contract summarization).  
- **n8n Foundations:**  
  - Screenshots of n8n interface: creating credentials, HTTP Request node, data passing via expressions.  
- **Personal Assistant Agent Design:**  
  - Diagram: Webhook → LLM → Google Sheets logging → Gmail notifications.  
  - Example Prompt: “You are a Chicago real estate expert…“  
- **Lead Generator Agent Design:**  
  - Diagram: Cron → HTTP Request (Redfin CSV) → Filter → Google Sheets → Gmail summary.  
  - Filtering Logic: e.g., price < $500k, bedrooms ≥ 3.  
- **RAG Agent Design:**  
  - Diagram: Webhook → Pinecone Query → LLM → JSON output.  
  - Prompt: Instruct LLM to cite clauses verbatim from indexed documents.  
- **Market Analysis Agent Design:**  
  - Diagram: Cron → CSV to JSON → Function (metrics) → Google Sheets → Email report.  
  - JS Snippet: Calculate average price and days on market.  
- **Social Media Agent Design:**  
  - Diagram: Cron → Google Sheets → LLM → Google Docs → (optional) Social API.  
  - Prompt: “Write a 200-character LinkedIn post highlighting these 3 new listings…”  
- **Advanced Architectures & Error Handling:**  
  - Multi-agent patterns: Sequential vs. orchestrator workflows.  
  - Error handling: IF nodes, retry settings, Slack/email alerts.  
- **Resources & Next Steps:**  
  - GitHub template repo: [RE-Agent-Templates](https://github.com/user/RE-Agent-Templates)  
  - n8n Community Forum: [community.n8n.io](https://community.n8n.io/)  
  - n8n Blog: [blog.n8n.io](https://blog.n8n.io/)  
  - n8n Documentation: [docs.n8n.io](https://docs.n8n.io/)

---

## 7. Example n8n Template Files (Detailed)
- **RE Personal Assistant Agent.json:**  
  - Webhook trigger, LLM node, Google Sheets append, Gmail send.  
- **RE Lead Generator Agent.json:**  
  - Cron trigger, HTTP Request (Redfin CSV), CSV to JSON, Filter, Google Sheets, Gmail.  
- **RE RAG Contract Agent.json:**  
  - Webhook trigger, Pinecone Query, LLM Chat Completion, JSON output or email.  
- **RE Market Analysis Agent.json:**  
  - Cron trigger, CSV to JSON, Function (metrics), Google Sheets summary, optional email.  
- **RE Social Media Agent.json:**  
  - Cron trigger, Google Sheets “Get Rows,” LLM post generation, Google Docs draft, (optional) Social API post.

---

## 8. Tips for Customizing Content for Real Estate Professionals (Expanded)
- **Terminology & Prompts:**  
  - Use industry jargon: “CAP rate,” “ARV,” “MLS ID,” “escrow,” “due diligence.”  
  - Real-world examples: “Buyer seeks 3-bedroom under $400k in Logan Square.”  
- **Data Sources:**  
  - Differentiate public endpoints (Redfin CSV exports) vs. paid APIs (Zillow, Realtor.com).  
  - Recommend sample CSV datasets if attendees lack API access.  
- **Use Case Scenarios:**  
  - Lead Nurturing: Tag leads (Hot, Warm, Cold) using thresholds or sentiment analysis.  
  - Contract Review & Compliance: Verify RAG outputs against source documents; use verbatim citations.  
  - Market Reporting: Automate weekly/monthly summaries to brokerage managers.  
- **Common Pitfalls & Solutions:**  
  - Prompt Hallucinations: Provide chunked context, require verbatim citations.  
  - API Rate Limits: Use IF nodes to detect 429 responses and implement delays/backoff.  
  - Credential Management: Use n8n’s credentials store, avoid hardcoding keys.

---

## 9. Post-Course Deliverables & Follow-Up (Expanded)
- **Shared Template Repository:**  
  - GitHub link: [RE-Agent-Templates](https://github.com/user/RE-Agent-Templates) (JSON workflows, sample data, README).  
- **Office-Hours Sessions:**  
  - Two 30-minute Zoom sessions the week after June 14 for troubleshooting and Q&A.  
- **Community Invitation:**  
  - Join n8n Community Forum: [community.n8n.io](https://community.n8n.io/) to share workflows and get help.  
- **Feedback Survey:**  
  - Google Form link to gather feedback on content, pacing, and future workshop ideas.  
- **Certificate of Completion (Optional):**  
  - Provide digital certificates or badges for attendees completing all sessions.

---

Thank you for exploring this course outline. Feel free to clone this repository and modify the templates to fit your real estate AI automation needs!
