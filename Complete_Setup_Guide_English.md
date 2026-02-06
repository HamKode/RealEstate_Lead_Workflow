# Real Estate Lead Qualification Bot - Professional Setup Guide (Gemini AI)

## Executive Summary

This comprehensive guide provides step-by-step instructions for implementing an AI-powered real estate lead qualification and automation system using n8n workflow automation platform with Google Gemini AI. The system automatically captures, qualifies, and nurtures leads while providing agents with actionable market insights.

## System Architecture Overview

**Target Market:** USA Real Estate Industry  
**Platform:** n8n Workflow Automation  
**AI Provider:** Google Gemini (gemini-1.5-flash/pro)  
**Integration Points:** Gmail, Google Calendar, Slack, CRM Systems  

### Core Functionality:
- Automated lead capture from multiple sources
- AI-powered lead qualification and scoring
- Intelligent email marketing automation
- Calendar scheduling for high-priority leads
- Real-time agent notifications
- Market intelligence generation

---

## Prerequisites

### Required Accounts & Services:
1. **n8n Cloud Account** or Self-hosted n8n instance
2. **Google AI Studio Account** with Gemini API access
3. **Gmail Account** with API access enabled
4. **Google Cloud Console** project with Calendar API
5. **Slack Workspace** with bot permissions
6. **Domain/Hosting** for webhook endpoints

### Technical Requirements:
- Basic understanding of workflow automation
- Access to Google Cloud Console
- Administrative access to Slack workspace
- Email marketing compliance knowledge

---

## Step 1: Initial n8n Setup

### 1.1 Workspace Configuration
1. Create new n8n workflow named "Real Estate Lead Qualification"
2. Import the provided JSON workflow file
3. Verify all nodes are properly positioned
4. Check node connections and data flow

### 1.2 Environment Variables
Configure the following environment variables in n8n:
```
GEMINI_API_KEY=your_gemini_api_key
GMAIL_CLIENT_ID=your_gmail_client_id
GMAIL_CLIENT_SECRET=your_gmail_client_secret
SLACK_BOT_TOKEN=your_slack_bot_token
```

---

## Step 2: Google Gemini API Configuration

### 2.1 Account Setup
1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Sign in with Google account
3. Create new API key for Gemini
4. Copy and securely store the API key

### 2.2 n8n Integration
1. Navigate to **Credentials** in n8n
2. Create new **Google Gemini API** credential
3. Enter API key from Google AI Studio
4. Test connection and save

### 2.3 Model Configuration
For each Gemini Chat Model node, configure:
- **Model:** `gemini-1.5-flash` or `gemini-1.5-pro`
- **Temperature:** 0.3 (Lead Enrichment), 0.7 (Message Generation)
- **Max Output Tokens:** 1000
- **Safety Settings:** Block only high-risk content

---

## Step 3: Gmail API Setup

### 3.1 Google Cloud Console Configuration
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create new project or select existing
3. Enable **Gmail API** and **Google Calendar API**
4. Navigate to **Credentials** → **Create Credentials** → **OAuth 2.0 Client IDs**

### 3.2 OAuth Configuration
```
Application Type: Web Application
Authorized JavaScript Origins: https://your-n8n-instance.com
Authorized Redirect URIs: https://your-n8n-instance.com/rest/oauth2-credential/callback
```

### 3.3 n8n Gmail Credentials
1. Create **Gmail OAuth2 API** credential in n8n
2. Enter Client ID and Client Secret
3. Complete OAuth authorization flow
4. Test email sending capability

---

## Step 4: Google Calendar Integration

### 4.1 Calendar API Setup
1. Ensure Google Calendar API is enabled in Cloud Console
2. Use same OAuth credentials as Gmail
3. Configure calendar permissions for event creation

### 4.2 Calendar Node Configuration
```
Calendar: Primary (or specific calendar ID)
Event Duration: 30 minutes
Default Start Time: Next business day, 10:00 AM
Attendee Notifications: Enabled
```

---

## Step 5: Slack Integration

### 5.1 Slack App Creation
1. Visit [Slack API](https://api.slack.com/apps)
2. Create new app for your workspace
3. Configure **Bot Token Scopes:**
   - `chat:write`
   - `channels:read`
   - `groups:read`

### 5.2 Bot Installation
1. Install app to workspace
2. Copy **Bot User OAuth Token**
3. Add bot to desired notification channel
4. Test message posting capability

---

## Step 6: AI Agent Configuration with Gemini

### 6.1 Lead Enrichment Agent
**System Message:**
```
You are a real estate lead qualification expert. Analyze the lead data and provide enrichment in JSON format with: 
- buyer_type (investor/first_time/renter)
- estimated_budget_range 
- buying_timeline (immediate/3months/6months/1year)
- lead_quality_score (0-100)
- key_insights

Always respond in valid JSON format only.
```

**Gemini Model Settings:**
- Model: `gemini-1.5-flash`
- Temperature: 0.3
- Max Output Tokens: 500

### 6.2 High Priority Message Agent
**System Message:**
```
You are a professional real estate agent. Create a personalized, engaging message for a high-quality lead. Keep it conversational, mention specific details about their interest, and include a clear call-to-action to schedule a call. Maximum 150 words. Be professional but friendly.
```

**Gemini Model Settings:**
- Model: `gemini-1.5-flash`
- Temperature: 0.7
- Max Output Tokens: 200

### 6.3 Nurture Message Agent
**System Message:**
```
Create a nurturing email for a medium-quality real estate lead. Be helpful, provide value, and maintain interest without being pushy. Include market insights or tips. Maximum 120 words. Focus on building relationship and trust.
```

**Gemini Model Settings:**
- Model: `gemini-1.5-flash`
- Temperature: 0.7
- Max Output Tokens: 150

### 6.4 Follow-up Message Agent
**System Message:**
```
Create a friendly follow-up message for a real estate lead who hasn't responded. Be helpful, not pushy. Offer value like market updates or property alerts. Maximum 100 words. Show genuine interest in helping them find their dream property.
```

**Gemini Model Settings:**
- Model: `gemini-1.5-flash`
- Temperature: 0.7
- Max Output Tokens: 120

### 6.5 Market Insights Agent
**System Message:**
```
You are a real estate market analyst. Provide actionable insights for an agent about this lead including: property recommendations, market trends for their area, pricing insights, and closing strategies. Be specific and data-driven. Format as bullet points. Maximum 200 words.
```

**Gemini Model Settings:**
- Model: `gemini-1.5-pro`
- Temperature: 0.4
- Max Output Tokens: 300

---

## Step 7: Node Configuration Changes for Gemini

### 7.1 Replace OpenRouter LLM Nodes
Replace all `@n8n/n8n-nodes-langchain.lmChatOpenRouter` nodes with:
- **Node Type:** `@n8n/n8n-nodes-langchain.lmChatGoogleGemini`
- **Credentials:** Google Gemini API
- **Model Selection:** Based on use case (flash for speed, pro for complex analysis)

### 7.2 Updated Node Names
- `Agent_1` → `Gemini_Lead_Enrichment`
- `Agent_2 High priority` → `Gemini_High_Priority`
- `Agent_3 Nurture` → `Gemini_Nurture`
- `Agent_4 Follow up` → `Gemini_Follow_up`
- `Agent_5 Market Insights` → `Gemini_Market_Insights`

### 7.3 Connection Updates
Ensure all AI Agent nodes are connected to their respective Gemini Chat Model nodes:
- AI Lead Enrichment → Gemini_Lead_Enrichment
- AI High Priority Message Agent → Gemini_High_Priority
- AI Nurture Message Agent → Gemini_Nurture
- AI Follow up Message Agent → Gemini_Follow_up
- AI Market Insights Agent → Gemini_Market_Insights

---

## Step 8: Email Template Configuration

### 8.1 High Priority Email Template
```
Subject: 🏠 Your {{ $json.location }} Property Search - Let's Connect!

Hi {{ $json.name }},

{{ $('AI High_Priority Message Agent').first().json.output }}

I noticed you're looking for {{ $json.property_interest }} in {{ $json.location }}. Based on your timeline of {{ $json.buying_timeline }}, I believe we can find you the perfect property within your budget range.

Here's what I can offer:
✅ Exclusive property listings in {{ $json.location }}
✅ Market insights and pricing analysis
✅ Personalized property recommendations
✅ Free consultation call scheduled for tomorrow

I've already scheduled a 30-minute consultation call for us tomorrow. You'll receive a calendar invite shortly.

Looking forward to helping you find your dream property!

Best regards,
Your Real Estate Expert
📞 (555) 123-4567
📧 agent@realestate.com
```

### 8.2 Nurture Email Template
```
Subject: 📈 {{ $json.location }} Market Update & Your Property Journey

Hi {{ $json.name }},

{{ $('AI Nurture Message Agent1').first().json.output }}

I hope you're doing well! I wanted to share some valuable insights about the {{ $json.location }} real estate market that might interest you.

🏡 Current Market Trends in {{ $json.location }}:
• Property values have shown steady growth
• Inventory levels are favorable for buyers
• Interest rates remain competitive
• Best time to buy is typically in the next 3-6 months

Since you expressed interest in {{ $json.property_interest }}, here are some tips:
✅ Get pre-approved for financing early
✅ Research neighborhood amenities
✅ Consider future resale value
✅ Don't rush - the right property will come

Stay in touch,
Your Real Estate Advisor
```

### 8.3 Follow-up Email Template
```
Subject: 🔔 Quick Check-in - {{ $json.location }} Properties Update

Hi {{ $json.name }},

{{ $('AI Follow up Message Agent').first().json.output }}

🆕 What's New This Week:
• 3 new {{ $json.property_interest }} listings in {{ $json.location }}
• Updated market analysis for your area
• New financing options available
• Exclusive off-market opportunities

Just reply with "YES" if you'd like me to send you the latest {{ $json.property_interest }} listings in {{ $json.location }}, or "PAUSE" if you'd prefer to hear from me less frequently.

Best regards,
Your Real Estate Partner
```

---

## Step 9: Workflow Testing with Gemini

### 9.1 Test Data Preparation
Create test scenarios with varying lead quality scores:

**High Quality Lead (Score: 85+)**
```json
{
  "name": "John Smith",
  "email": "john.smith@example.com",
  "phone": "+1-555-123-4567",
  "location": "Austin, TX",
  "property_type": "3BR Single Family Home",
  "budget": "$400,000-$500,000",
  "timeline": "immediate"
}
```

**Medium Quality Lead (Score: 50-69)**
```json
{
  "name": "Sarah Johnson",
  "email": "sarah.j@example.com",
  "phone": "+1-555-987-6543",
  "location": "Dallas, TX",
  "property_type": "2BR Condo",
  "budget": "$200,000-$300,000",
  "timeline": "6 months"
}
```

### 9.2 Gemini-Specific Testing
1. **Response Quality:** Verify Gemini generates coherent, relevant responses
2. **JSON Validation:** Ensure lead enrichment returns valid JSON
3. **Safety Filters:** Confirm content passes Gemini's safety guidelines
4. **Token Usage:** Monitor token consumption for cost optimization

### 9.3 Performance Monitoring
Monitor the following metrics:
- Lead processing time (target: <45 seconds with Gemini)
- AI response accuracy (>90% valid JSON)
- Email delivery rate (>95%)
- Calendar event creation success (>98%)
- Gemini API quota usage

---

## Step 10: Production Deployment

### 10.1 Security Configuration
1. **API Key Management:** Store Gemini API key securely
2. **Webhook Security:** Implement request validation
3. **Rate Limiting:** Configure appropriate API limits
4. **Error Handling:** Set up comprehensive error logging

### 10.2 Monitoring & Alerts
1. **Execution Monitoring:** Set up workflow failure alerts
2. **Performance Tracking:** Monitor processing times
3. **Cost Management:** Track Gemini API usage and costs
4. **Quality Assurance:** Regular AI response auditing

### 10.3 Backup & Recovery
1. **Workflow Backup:** Export workflow JSON regularly
2. **Credential Backup:** Secure credential storage
3. **Data Retention:** Configure appropriate data retention policies

---

## Step 11: Gemini-Specific Optimizations

### 11.1 Cost Optimization
- Use `gemini-1.5-flash` for simple tasks (faster, cheaper)
- Use `gemini-1.5-pro` for complex analysis (better quality)
- Implement response caching for similar queries
- Monitor token usage and optimize prompts

### 11.2 Performance Tuning
- Adjust temperature based on task requirements
- Optimize max output tokens for each use case
- Implement retry logic for API failures
- Use batch processing where possible

### 11.3 Safety & Compliance
- Configure appropriate safety settings
- Monitor for content policy violations
- Implement content filtering if needed
- Regular audit of AI-generated content

---

## Step 12: Advanced Customization

### 12.1 CRM Integration
Add CRM nodes for:
- **HubSpot:** Lead creation and status updates
- **Salesforce:** Opportunity management
- **Pipedrive:** Deal pipeline automation

### 12.2 Additional Channels
Extend lead capture to:
- **Facebook Lead Ads:** Direct API integration
- **Zillow Premier Agent:** Lead import automation
- **Realtor.com:** Lead management integration

### 12.3 Advanced Analytics
Implement tracking for:
- Lead source attribution
- Conversion rate optimization
- ROI measurement
- Agent performance metrics

---

## Troubleshooting Guide

### Common Issues & Solutions

**Issue:** Gemini API not responding
- **Solution:** Check API key validity and quota limits

**Issue:** Safety filters blocking content
- **Solution:** Adjust safety settings or refine prompts

**Issue:** Inconsistent JSON responses
- **Solution:** Improve system prompts with examples

**Issue:** High token usage costs
- **Solution:** Optimize prompts and use appropriate models

**Issue:** Slow response times
- **Solution:** Use gemini-1.5-flash for faster responses

---

## Compliance & Best Practices

### 13.1 Email Marketing Compliance
- **CAN-SPAM Act:** Include unsubscribe links
- **GDPR Compliance:** Implement consent management
- **Data Privacy:** Secure handling of personal information

### 13.2 Real Estate Regulations
- **Fair Housing Act:** Ensure non-discriminatory practices
- **State Licensing:** Comply with local real estate laws
- **Lead Management:** Follow industry best practices

### 13.3 AI Ethics & Transparency
- **Disclosure:** Inform clients about AI usage
- **Bias Prevention:** Regular audit of AI responses
- **Human Oversight:** Maintain agent review processes

---

## Support & Maintenance

### 14.1 Regular Maintenance Tasks
- **Weekly:** Review Gemini response quality
- **Monthly:** Update prompts and optimize performance
- **Quarterly:** Audit workflow performance and costs

### 14.2 Scaling Considerations
- **High Volume:** Implement queue management
- **Multi-Agent:** Configure team-based routing
- **Geographic Expansion:** Localize content and regulations

---

## Conclusion

This Real Estate Lead Qualification Bot with Google Gemini provides a cost-effective, high-performance solution for automating lead management while maintaining personalized customer engagement. Gemini's advanced capabilities ensure accurate lead qualification and natural language generation for improved conversion rates.

The system scales efficiently and provides measurable ROI through improved lead conversion rates and agent productivity, while leveraging Google's cutting-edge AI technology.

For technical support or customization requests, please refer to the n8n documentation or contact your implementation team.

---

**Document Version:** 2.0 (Gemini Edition)  
**Last Updated:** January 2024  
**Target Audience:** Real Estate Professionals, USA Market  
**AI Provider:** Google Gemini