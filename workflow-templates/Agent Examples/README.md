# 🤖 n8n AI Agent Examples

## 📋 Overview

This workflow contains three standalone AI agent templates that showcase different levels of AI agent implementation in n8n. Each template demonstrates a specific pattern for building AI-powered workflows with varying capabilities and complexity.

## 🧩 Agent Templates Included

### 1. 🔄 Basic AI Agent Template

**Purpose:** Provides the simplest AI agent implementation with core components for basic conversational AI.

**Components:**
- Workflow trigger for execution
- Session ID generation for conversation tracking
- 🧠 Window Buffer Memory for context retention
- LLM integration (OpenRouter/GPT-4.1-mini)
- 💭 Think tool for reasoning
- AI Agent node with customizable system prompt

**Use Cases:**
- 💬 Simple Q&A systems
- 🔍 Basic decision-making
- Single-purpose conversational agents
- Introductory AI agent implementations

**Best Practices:**
- Keep system prompts concise and focused on a single domain
- Use the Think tool to break down complex reasoning steps
- Start with smaller context windows (4-8) for better performance
- 🧪 Test with various LLM models to find the right balance of speed and capability

### 2. 📊 AI Agent with Structured Output

**Purpose:** Extends the basic agent with structured output capabilities, enabling consistent data formatting.

**Components:**
- All components from the Basic Agent
- 📋 Structured Output Parser for enforcing JSON schema
- Enhanced system prompt with output format instructions

**Use Cases:**
- 📝 Data extraction from unstructured text
- 📋 Form filling and validation
- 🏷️ Categorization and classification tasks
- 🔌 API integration where consistent output format is required

**Best Practices:**
- Define clear JSON schemas with required fields and types
- Include example outputs in system prompts
- Use the Think tool to plan structured responses before generating
- ⚠️ Implement error handling for malformed outputs
- Test edge cases to ensure schema compliance

### 3. 🌐 Advanced AI Agent with HTTP Tools

**Purpose:** Comprehensive agent with external data access capabilities, allowing real-time information retrieval and processing.

**Components:**
- All components from previous templates
- 🔗 HTTP Request tool for external API calls
- 🔍 Web search capabilities
- Enhanced memory management
- Multi-step reasoning capabilities

**Use Cases:**
- ⏱️ Real-time data analysis
- 🔎 Web research automation
- 📄 Dynamic content generation
- 🧩 Complex decision-making with external information

**Best Practices:**
- 🔒 Carefully scope API access permissions
- ⏱️ Use rate limiting to prevent excessive API calls
- 💾 Implement caching for frequently accessed data
- Structure prompts to guide effective use of external tools
- Provide clear instructions on when to use which tool
- Test with various input complexities to ensure robustness

## 🔄 Integration Points

- **Language Models:** Compatible with OpenAI, Claude, and other models via OpenRouter
- **Memory Systems:** Uses n8n's built-in memory nodes for context management
- **External Services:** Can connect to any API through HTTP Request nodes
- **Output Formats:** Supports both conversational and structured data outputs

## ✨ Tips for Customization

1. **System Prompts:** The heart of agent behavior - customize these first to adapt the agent to your specific use case
2. **Tool Selection:** Add or remove tools based on your agent's specific needs
3. **Memory Configuration:** Adjust context window size based on conversation complexity
4. **Model Selection:** Balance between capability and cost with appropriate model choice
5. **Error Handling:** Add error handling nodes for production deployments
6. **Output Parsing:** Define custom schemas for your specific data requirements

## 🚀 Getting Started

1. Import this workflow into your n8n instance
2. Configure your API credentials for OpenRouter or other LLM providers
3. Test each agent template individually to understand its capabilities
4. Use these templates as starting points for your own custom AI agent workflows

## 📚 Additional Resources

- For more advanced AI agent patterns, see the RAG AI Agent and Personal Assistant with MCP templates
- Explore the Client Support Bot for a production-ready implementation
- See the Real Estate Market Expert for an example of domain-specific AI agents 