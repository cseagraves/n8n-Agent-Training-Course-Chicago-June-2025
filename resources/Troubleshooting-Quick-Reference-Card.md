# Troubleshooting Quick Reference Card

**n8n AI Agent Training Course - June 14th, 2025**  
**Quick Solutions for Common Issues**

---

## 🚨 Emergency Quick Fixes

### Workflow Not Triggering
- **Check:** Green "Active" toggle is ON
- **Check:** Webhook URL copied correctly from n8n
- **Check:** Test with manual trigger first
- **Fix:** Click "Execute Workflow" to test manually

### "Node has no data" Error
- **Cause:** Previous node returned empty results
- **Fix:** Add a "No Operation, do nothing" node between them
- **Alternative:** Use IF node to handle empty data paths

### API Connection Failed
- **Check:** Credentials are saved and selected in the node
- **Check:** API key is valid (test in Postman/browser)
- **Check:** Service isn't experiencing downtime
- **Fix:** Re-authenticate or create new credentials

---

## 🔧 Node-Specific Issues

### HTTP Request Node
| Problem | Solution |
|---------|----------|
| 401 Unauthorized | Check API key format, add "Bearer " prefix if needed |
| 404 Not Found | Verify endpoint URL, check API documentation |
| CORS Error | Can't be fixed in n8n, API must allow CORS |
| Timeout | Increase timeout in node settings (default 300s) |
| SSL Error | Disable "SSL Certificate Validation" for testing |

### AI Agent Node
| Problem | Solution |
|---------|----------|
| "No model selected" | Connect Chat Model node to AI Agent |
| Empty responses | Check system prompt, ensure it's instructional |
| Model timeout | Switch to faster model (gpt-4o-mini vs gpt-4o) |
| Too expensive | Use cheaper models for testing (gpt-4o-mini) |
| Hallucinations | Add specific constraints in system prompt |

### Google Services
| Problem | Solution |
|---------|----------|
| Permission denied | Share sheet/doc with service account email |
| File not found | Use file ID from URL, not file name |
| Rate limit exceeded | Add Wait node between API calls (2-3 seconds) |
| Auth refresh failed | Delete and recreate Google credentials |

### Vector Store (Pinecone)
| Problem | Solution |
|---------|----------|
| Index not found | Verify index name matches exactly (case-sensitive) |
| Dimension mismatch | Use 1536 for OpenAI embeddings |
| Quota exceeded | Check Pinecone usage dashboard |
| Slow responses | Reduce number of vectors retrieved |

---

## 🐛 Data Flow Issues

### JSON Structure Problems
```javascript
// Wrong - trying to access nested data incorrectly
{{ $json.contact.email }}

// Right - check data structure first
{{ $json.body.contact.email }}

// Safe access with fallback
{{ $json.contact?.email || $json.email }}
```

### Array vs Single Item Confusion
```javascript
// If data is an array, access first item:
{{ $json[0].email }}

// If expecting single item but got array:
Use "Split Out" node to convert array to multiple items
```

### Missing Required Fields
```javascript
// Check if field exists before using:
{{ $json.email || "no-email@example.com" }}

// Set default values in Set node:
email: {{ $json.email || "N/A" }}
```

---

## ⚡ Performance Issues

### Slow Workflow Execution
1. **Optimize API calls:** Batch requests when possible
2. **Use pagination:** Don't fetch all data at once
3. **Add Wait nodes:** Prevent rate limiting (1-2 seconds)
4. **Minimize AI calls:** Cache responses when appropriate
5. **Use simpler models:** gpt-4o-mini vs gpt-4o for testing

### Memory Issues
- **Limit data size:** Use pagination or filtering
- **Remove unnecessary fields:** Use Set node to clean data
- **Avoid loops:** Use Split Out for array processing instead

### Rate Limiting
- **Google APIs:** Max 100 requests/100 seconds/user
- **OpenAI:** Varies by model and plan
- **Solution:** Add Wait nodes between calls

---

## 🔐 Credential Issues

### Google Service Account
```bash
# Check service account email format:
service-account-name@project-id.iam.gserviceaccount.com

# Required permissions:
- Editor access to Google Sheets/Docs
- Gmail API enabled in Google Cloud Console
```

### OpenRouter/AI APIs
```bash
# API Key format check:
OpenRouter: sk-or-v1-xxx...
OpenAI: sk-xxx...
Anthropic: sk-ant-xxx...

# Common issues:
- Missing "Bearer " prefix in HTTP headers
- Using wrong base URL for OpenRouter
- Insufficient credits in account
```

### Webhook Security
```bash
# Secure webhook endpoints:
- Use HTTPS URLs only
- Add authentication headers if possible
- Validate incoming data structure
```

---

## 📱 Environment-Specific Issues

### n8n Cloud vs Self-Hosted
| Issue | Cloud Solution | Self-Hosted Solution |
|-------|----------------|---------------------|
| Timeout limits | Use cloud timeouts (max 2 min) | Configure custom timeouts |
| File size limits | Max 16MB files | Configure server limits |
| Custom nodes | Request from n8n | Install via npm |
| Database access | Not available | Direct SQLite/PostgreSQL access |

### Browser-Specific Issues
- **Chrome:** Works best, use for demos
- **Safari:** May have CORS issues with some APIs
- **Firefox:** Generally works well
- **Solution:** Test in Chrome if issues arise

---

## 🎯 Real Estate Specific Issues

### MLS Integration
- **Problem:** Rate limiting from MLS providers
- **Solution:** Cache data, update only when necessary
- **Best Practice:** Morning batch updates vs real-time

### Lead Source Integration
```javascript
// Common webhook payloads vary:
// Zillow: { lead: { contact: { email } } }
// Realtor.com: { prospect: { email_address } }
// Facebook: { field_data: [{ name: "email", values: ["email@domain.com"] }] }

// Solution: Use multiple IF nodes to handle different structures
```

### Property Data Inconsistency
- **Address formats:** Standardize with Google Geocoding API
- **Price formats:** Remove commas, handle "$" symbols
- **Date formats:** Use moment.js in Function node

---

## 📞 Getting Help

### Before Asking for Help
1. ✅ Check this troubleshooting guide
2. ✅ Test with manual trigger
3. ✅ Check execution log for errors
4. ✅ Verify credentials and permissions
5. ✅ Test individual nodes one by one

### How to Ask for Help
1. **Share workflow JSON** (remove sensitive data)
2. **Include error message** (full text)
3. **Describe expected vs actual behavior**
4. **Mention what you've already tried**

### Resources for Help
- **Course Instructor:** cayman@caiyman.ai
- **n8n Community:** https://community.n8n.io/
- **n8n Documentation:** https://docs.n8n.io/
- **Course Forum:** [Link to be provided]

---

## 🔄 Common Workflow Patterns

### Error Handling Pattern
```
Trigger → Try (your main workflow) 
       → Set Error Info (if fails)
       → Send Error Notification
```

### Data Validation Pattern
```
HTTP Request → IF (check if data exists)
            → True: Process Data
            → False: Set Default/Send Alert
```

### API Retry Pattern
```
HTTP Request → IF (success?)
            → False: Wait (5 sec) → HTTP Request (retry)
            → True: Continue workflow
```

---

## 🎪 Demo Day Preparation

### Pre-Demo Checklist
- [ ] All workflows tested and working
- [ ] Credentials verified and active
- [ ] Test data prepared
- [ ] Backup workflows exported
- [ ] Internet connection stable
- [ ] Browser cleared of extensions

### During Demo Issues
1. **Stay calm** - most issues are simple fixes
2. **Use manual trigger** if webhook fails
3. **Have backup static data** ready
4. **Switch to simpler example** if needed
5. **Explain the issue** as learning opportunity

---

*Keep this card handy during development and always test thoroughly before going live!*

© 2025 Caiyman AI LLC. All rights reserved. 