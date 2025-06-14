# 🤖 n8n RAG AI Agent for OKRC Contracts

## 📋 Overview

This workflow implements a Retrieval-Augmented Generation (RAG) AI agent specialized for querying and analyzing OKRC contract documents. The system automatically ingests documents from Google Drive, processes them into embeddings stored in Pinecone, and provides an intelligent interface for natural language queries about contract information.

## 🎯 Purpose

The RAG AI Agent for OKRC Contracts serves as a knowledge management system that:
- 📄 Automatically processes contract documents when added to a designated Google Drive folder
- 🔍 Creates searchable vector embeddings of document content
- 💬 Provides natural language query capabilities for contract information
- ✅ Delivers accurate, context-aware responses based on the actual content of contracts

## 🔌 Integrations & Tools

### Document Processing Pipeline
- **📁 Google Drive Triggers**: Monitor folder for new/updated contract documents
- **⬇️ Google Drive Download**: Retrieve document files for processing
- **📝 Default Data Loader**: Extract text content from various document formats
- **✂️ Text Splitter**: Chunk documents into manageable segments
- **🧠 OpenAI Embeddings**: Generate vector embeddings of document chunks
- **💾 Pinecone Vector Store**: Store and index document embeddings

### Query Interface
- **🔍 Pinecone Vector Store (Retrieval)**: Search for relevant document chunks
- **🧠 Window Buffer Memory**: Maintain conversation context
- **🤖 OpenRouter LLM**: Power the AI agent with advanced language capabilities
- **💬 Chat Interface**: Provide natural language interaction with the system

## ⚙️ Setup Instructions

1. **Google Cloud & API Setup**:
   - Create a Google Cloud project
   - Enable Vertex AI API & Google Drive API
   - Set up OAuth2 consent screen
   - Configure credentials

2. **OpenAI API Configuration**:
   - Obtain API key from OpenAI Developer Platform
   - Add billing method for API usage

3. **Pinecone Setup**:
   - Create a Pinecone account
   - Get API key from dashboard
   - Create index: `rag-orec-contracts-data-lake-small-embeddings`
   - Use dimension: 1536 (for OpenAI embeddings)

4. **Google Drive Configuration**:
   - Create a dedicated folder for OKRC contracts
   - Note the folder ID from the URL
   - Update folder ID in Google Drive trigger nodes

5. **n8n Credentials**:
   - Configure Google Drive OAuth2, OpenAI API, and Pinecone API credentials
   - Test each connection

## 📖 Usage Guidelines

### 📄 Document Management
- Upload contract documents to the designated Google Drive folder
- Supported formats include PDF, DOCX, TXT, and other text-based formats
- Large documents will be automatically chunked for optimal processing
- Allow processing time before documents become searchable

### 🔍 Querying Contracts
- Use natural language to ask questions about contracts
- Be specific about the information you're seeking
- Reference specific contract types, parties, or clauses when possible
- Follow up with clarifying questions if needed

**Example Queries:**
- "What are the termination clauses in the Johnson contract?"
- "Summarize the payment terms in our vendor agreements"
- "Find all contracts that expire in the next 90 days"
- "What are our obligations regarding confidentiality in the ABC Corp agreement?"

## ✅ Best Practices

### 📄 Document Preparation
- Use consistent naming conventions for contract files
- Ensure documents are properly OCR'd if scanned
- Consider adding metadata in document headers for better context
- Keep file sizes manageable (split very large contracts if needed)

### 🔍 Query Optimization
- Start with specific questions rather than broad queries
- Use contract identifiers or party names when available
- Break complex questions into simpler sequential queries
- Use follow-up questions to drill down into details

### ⚙️ System Management
- Monitor vector store size and usage
- Periodically review and clean up outdated documents
- Update embeddings when contracts are amended
- Consider separate namespaces for different contract categories

## ❓ Troubleshooting

- **📄 Document Not Found**: Ensure document is in the correct Drive folder and processing has completed
- **❌ Inaccurate Responses**: Check if document quality is sufficient; consider re-uploading with better OCR
- **⏱️ Processing Delays**: Large documents may take longer to process; check workflow execution logs
- **📏 Context Limitations**: Very long contracts may need multiple queries to extract all relevant information

## 🔧 Advanced Customization

- **📏 Chunk Size Adjustment**: Modify the text splitter parameters for different document types
- **🧠 Embedding Model**: Change the embedding model for different performance characteristics
- **🏷️ Namespace Strategy**: Implement multiple namespaces for different contract categories
- **💬 Query Enhancement**: Add pre-processing for specialized legal terminology

## 🔒 Security Considerations

- Ensure proper access controls for the Google Drive folder
- Restrict workflow access to authorized personnel
- Consider encryption for highly sensitive contract data
- Implement user authentication for the query interface

## ⚡ Performance Optimization

- Adjust chunk size and overlap based on document characteristics
- Consider using more efficient embedding models for large document collections
- 💾 Implement caching for frequently accessed information
- Use batch processing for document ingestion 