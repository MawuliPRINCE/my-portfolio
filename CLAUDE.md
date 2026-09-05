# CLAUDE.md
## Claude AI Integration Guide

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Purpose:** Claude-specific implementation guide for portfolio automation

---

## Quick Start

### What is Claude?
Claude is an AI assistant created by Anthropic that can:
- Analyze and process text/data
- Generate high-quality content
- Solve complex problems
- Follow detailed instructions reliably
- Maintain context across conversations

### Why Claude for This Portfolio?
- ✅ Excellent for content analysis and generation
- ✅ Strong at following structured instructions
- ✅ Reliable for JSON processing
- ✅ Good reasoning for validation tasks
- ✅ Affordable API pricing
- ✅ Easy integration with GitHub workflows

---

## API Setup

### 1. Get API Key

```bash
# Visit: https://console.anthropic.com
# Create account
# Generate API key
# Copy key (starts with sk-)
```

### 2. Store in GitHub Secrets

```bash
# GitHub Repo Settings → Secrets and variables → Actions
# Name: CLAUDE_API_KEY
# Value: sk-...

# Also add (optional):
# CLAUDE_MODEL: claude-3-5-sonnet-20241022
# CLAUDE_MAX_TOKENS: 2048
```

### 3. Install SDK

```bash
# JavaScript/Node.js
npm install @anthropic-ai/sdk

# Python
pip install anthropic

# Bash
curl https://console.anthropic.com/docs/quickstart
```

---

## Available Models

### Claude 3 Family (Recommended)

| Model | Speed | Cost | Best For |
|---|---|---|---|
| claude-3-5-sonnet-20241022 | Fast ⚡ | Low 💰 | General tasks, content generation |
| claude-3-opus-20250219 | Slow 🐢 | High 💸 | Complex reasoning, detailed analysis |
| claude-3-haiku-20250307 | Very Fast ⚡⚡ | Very Low 💵 | Simple tasks, high volume |

**Recommendation:** Use `claude-3-5-sonnet-20241022` for most portfolio tasks.

---

## Basic Integration

### 1. Simple Message API Call

```javascript
const Anthropic = require("@anthropic-ai/sdk");

const client = new Anthropic({
  apiKey: process.env.CLAUDE_API_KEY
});

async function askClaude(prompt) {
  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [
      {
        role: "user",
        content: prompt
      }
    ]
  });

  return message.content[0].text;
}

// Usage
const response = await askClaude("Generate an Instagram caption for a branding project");
console.log(response);
```

### 2. Multi-Turn Conversation

```javascript
async function multiTurnConversation() {
  const messages = [];

  // First message
  messages.push({
    role: "user",
    content: "I have a design project. Can you help analyze it?"
  });

  const response1 = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: messages
  });

  const assistantResponse = response1.content[0].text;
  messages.push({
    role: "assistant",
    content: assistantResponse
  });

  // Follow-up message
  messages.push({
    role: "user",
    content: "It's a luxury jewelry brand. What should I focus on?"
  });

  const response2 = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: messages
  });

  return response2.content[0].text;
}
```

### 3. Structured Input/Output

```javascript
async function validateProjectData(projectData) {
  const prompt = `
You are a portfolio data validator. Analyze this project and respond with JSON.

Project Data:
${JSON.stringify(projectData, null, 2)}

Validation Checklist:
1. Is title 3-60 characters?
2. Are all required fields present?
3. Are tags from approved list?
4. Is description formatted properly?
5. Are media URLs valid?

Respond ONLY with JSON in this format:
{
  "valid": boolean,
  "errors": ["list of errors"],
  "warnings": ["list of warnings"],
  "suggestions": ["improvement suggestions"],
  "score": 0-100
}
`;

  const response = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [{ role: "user", content: prompt }]
  });

  try {
    return JSON.parse(response.content[0].text);
  } catch (e) {
    console.error("Failed to parse Claude response as JSON");
    return null;
  }
}
```

---

## Use Case Examples

### 1. Generate SEO Metadata

```javascript
async function generateSEOMetadata(project) {
  const prompt = `
Generate SEO metadata for this design project.

Project Title: ${project.title}
Project Description: ${project.description}
Project Type: ${project.tags.join(", ")}

Generate ONLY a JSON response (no markdown, no extra text):
{
  "seoTitle": "SEO title (50-60 chars)",
  "seoDescription": "Meta description (150-160 chars)",
  "keywords": ["keyword1", "keyword2", "keyword3"],
  "ogTitle": "Open Graph title",
  "ogDescription": "Open Graph description"
}`;

  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 512,
    messages: [{ role: "user", content: prompt }]
  });

  return JSON.parse(message.content[0].text);
}

// Usage
const project = {
  title: "MIKITA Fine Jewelry",
  description: "Luxury jewelry brand identity...",
  tags: ["Branding", "Luxury", "Jewelry"]
};

const seo = await generateSEOMetadata(project);
console.log(seo.seoTitle);  // "MIKITA Luxury Jewelry Brand Identity | Designer"
```

### 2. Auto-Generate Social Media Posts

```javascript
async function generateSocialPost(project, platform = "instagram") {
  const platformGuidelines = {
    instagram: {
      max_length: 2200,
      tone: "engaging, visual-focused",
      hashtags: 10-15
    },
    linkedin: {
      max_length: 3000,
      tone: "professional, case-study focused",
      hashtags: 3-5
    },
    twitter: {
      max_length: 280,
      tone: "punchy, witty",
      hashtags: 2-3
    }
  };

  const guidelines = platformGuidelines[platform];

  const prompt = `
You are a social media expert. Generate a ${platform} post for this design project.

Project Title: ${project.title}
Description: ${project.description}
Tags: ${project.tags.join(", ")}

Guidelines:
- Platform: ${platform}
- Max length: ${guidelines.max_length} characters
- Tone: ${guidelines.tone}
- Include ${guidelines.hashtags} hashtags
- Add relevant emojis
- Include portfolio link

Respond ONLY with JSON:
{
  "caption": "full caption text",
  "hashtags": "#tag1 #tag2",
  "emojis": "👍 🎨 ✨",
  "cta": "call-to-action text"
}`;

  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [{ role: "user", content: prompt }]
  });

  return JSON.parse(message.content[0].text);
}

// Usage
const post = await generateSocialPost(project, "instagram");
console.log(post.caption);
```

### 3. Analyze Portfolio Performance

```javascript
async function analyzePortfolioPerformance(analyticsData, projectList) {
  const prompt = `
Analyze this portfolio's performance data and provide insights.

Analytics (Last 30 Days):
${JSON.stringify(analyticsData, null, 2)}

Projects:
${JSON.stringify(projectList, null, 2)}

Provide analysis in JSON format:
{
  "summary": "Overall performance summary",
  "top_performers": [
    { "project": "name", "reason": "why it performs well" }
  ],
  "opportunities": ["improvement opportunity 1", "opportunity 2"],
  "recommendations": [
    { "action": "specific action", "impact": "expected impact", "effort": "easy/medium/hard" }
  ],
  "trending": {
    "tags": ["trending tags"],
    "skills": ["in-demand skills"],
    "client_types": ["types of clients to pursue"]
  }
}`;

  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 2048,
    messages: [{ role: "user", content: prompt }]
  });

  return JSON.parse(message.content[0].text);
}
```

### 4. Review & Improve Project Descriptions

```javascript
async function improveProjectDescription(project) {
  const prompt = `
Review and improve this portfolio project description.

Current Description:
${project.description}

Improvements needed:
1. Add clear structure (Challenge → Solution → Result)
2. Enhance with more specific details
3. Improve SEO with relevant keywords
4. Make it more engaging for potential clients
5. Add quantifiable outcomes if possible

Respond ONLY with JSON:
{
  "improved_description": "enhanced HTML description",
  "key_improvements": ["improvement 1", "improvement 2"],
  "keywords": ["seo keyword 1", "keyword 2"],
  "readability_score": 1-10,
  "estimated_read_time": "X minutes"
}`;

  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [{ role: "user", content: prompt }]
  });

  return JSON.parse(message.content[0].text);
}
```

### 5. Generate Image Alt Text

```javascript
async function generateAltText(imageDescription, projectContext) {
  const prompt = `
Generate accessible alt text for a portfolio image.

Image Context:
- Project: ${projectContext.project}
- Sector: ${projectContext.sector}
- Image shows: ${imageDescription}

Requirements:
- Must be concise (under 125 characters)
- Should be descriptive and meaningful
- Include relevant keywords
- Suitable for screen readers

Respond ONLY with:
{
  "altText": "description"
}`;

  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 256,
    messages: [{ role: "user", content: prompt }]
  });

  return JSON.parse(message.content[0].text).altText;
}
```

---

## GitHub Actions Integration

### Example Workflow: Content Validation

```yaml
# .github/workflows/claude-validate-content.yml
name: Claude Content Validator

on:
  push:
    paths:
      - 'projects.json'
      - 'site-about.json'
  workflow_dispatch:

jobs:
  validate:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm install @anthropic-ai/sdk
      
      - name: Run Claude validation
        env:
          CLAUDE_API_KEY: ${{ secrets.CLAUDE_API_KEY }}
        run: |
          node scripts/validateWithClaude.js
      
      - name: Commit improvements
        if: success()
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "Claude: Auto-validated and improved content"
          file_pattern: 'projects.json site-about.json'
```

### Example Script: Validation Agent

```javascript
// scripts/validateWithClaude.js
const fs = require("fs");
const Anthropic = require("@anthropic-ai/sdk");

const client = new Anthropic({
  apiKey: process.env.CLAUDE_API_KEY
});

async function validateAllProjects() {
  const projectsData = JSON.parse(fs.readFileSync("projects.json"));
  
  const validationResults = {
    timestamp: new Date().toISOString(),
    total: projectsData.length,
    valid: 0,
    issues: [],
    improvements: []
  };

  for (const project of projectsData) {
    const prompt = `
Validate this design project entry:
${JSON.stringify(project, null, 2)}

Check:
1. All required fields present
2. Proper formatting
3. Content quality
4. SEO potential

Respond with JSON:
{
  "valid": boolean,
  "issues": ["any issues"],
  "improvements": ["suggested improvements"],
  "seoScore": 0-100
}`;

    try {
      const message = await client.messages.create({
        model: "claude-3-5-sonnet-20241022",
        max_tokens: 512,
        messages: [{ role: "user", content: prompt }]
      });

      const result = JSON.parse(message.content[0].text);
      
      if (result.valid) {
        validationResults.valid++;
      } else {
        validationResults.issues.push({
          project: project.title,
          issues: result.issues
        });
      }

      if (result.improvements.length > 0) {
        validationResults.improvements.push({
          project: project.title,
          improvements: result.improvements
        });
      }
    } catch (error) {
      console.error(`Error validating ${project.title}:`, error.message);
    }
  }

  console.log(JSON.stringify(validationResults, null, 2));
  
  // Save report
  fs.writeFileSync(
    "validation-report.json",
    JSON.stringify(validationResults, null, 2)
  );

  return validationResults;
}

validateAllProjects().catch(console.error);
```

---

## Cost Management

### Pricing Overview

```
Claude 3.5 Sonnet:
- Input:  $3 per 1M tokens
- Output: $15 per 1M tokens

Average Portfolio Task Costs:
- Validate 1 project:     ~$0.001
- Generate SEO metadata:  ~$0.002
- Create social post:     ~$0.003
- Analyze analytics:      ~$0.010
- Improve description:    ~$0.005

Monthly Budget Examples:
- 100 validations:   ~$0.10
- 50 social posts:   ~$0.15
- 10 deep analyses:  ~$0.10
- Total:             ~$0.35/month (very affordable)
```

### Cost Optimization Tips

```javascript
// 1. Use token counting before making expensive calls
const message = await client.messages.create({
  model: "claude-3-5-sonnet-20241022",
  max_tokens: 1024,
  messages: [/* ... */]
});

const usage = message.usage;
console.log(`Input: ${usage.input_tokens}, Output: ${usage.output_tokens}`);

// 2. Use cheaper model for simple tasks
// Use haiku instead of sonnet for straightforward tasks

// 3. Batch multiple tasks
// Process 5 projects at once instead of 5 separate calls

// 4. Set max_tokens appropriately
// Don't request 4096 tokens if you only need 256
```

### Monthly Cost Alert

```javascript
// Track spending
let monthlySpend = 0;
const MONTHLY_BUDGET = 50; // $50/month limit

async function callClaude(prompt, maxTokens = 1024) {
  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: maxTokens,
    messages: [{ role: "user", content: prompt }]
  });

  // Estimate cost
  const inputCost = (message.usage.input_tokens / 1_000_000) * 3;
  const outputCost = (message.usage.output_tokens / 1_000_000) * 15;
  const callCost = inputCost + outputCost;

  monthlySpend += callCost;

  if (monthlySpend > MONTHLY_BUDGET) {
    console.warn(`⚠️ Monthly budget exceeded: $${monthlySpend.toFixed(2)}`);
    // Stop making calls or alert user
  }

  return message;
}
```

---

## Error Handling

### Common Errors & Solutions

```javascript
// Error 1: Invalid API Key
try {
  const message = await client.messages.create(/* ... */);
} catch (error) {
  if (error.status === 401) {
    console.error("Invalid API key. Check CLAUDE_API_KEY secret.");
  }
}

// Error 2: Rate Limiting
async function callWithRetry(prompt, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await client.messages.create({
        model: "claude-3-5-sonnet-20241022",
        max_tokens: 1024,
        messages: [{ role: "user", content: prompt }]
      });
    } catch (error) {
      if (error.status === 429 && i < maxRetries - 1) {
        // Wait and retry
        await new Promise(resolve => setTimeout(resolve, 2000 * Math.pow(2, i)));
      } else {
        throw error;
      }
    }
  }
}

// Error 3: Invalid JSON Response
try {
  const result = JSON.parse(response.content[0].text);
} catch (error) {
  console.error("Claude didn't return valid JSON");
  // Try again or use fallback
}
```

---

## Advanced Features

### 1. Vision (Image Analysis) - Future

```javascript
// Coming soon: Analyze portfolio images
async function analyzeImage(imageUrl) {
  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [
      {
        role: "user",
        content: [
          {
            type: "image",
            source: {
              type: "url",
              url: imageUrl
            }
          },
          {
            type: "text",
            text: "Describe what you see in this design image."
          }
        ]
      }
    ]
  });

  return message.content[0].text;
}
```

### 2. System Messages (Custom Behavior)

```javascript
// Define Claude's behavior
async function callClaudeWithSystem(userPrompt, systemPrompt) {
  const message = await client.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    system: systemPrompt,  // Custom instructions
    messages: [
      {
        role: "user",
        content: userPrompt
      }
    ]
  });

  return message.content[0].text;
}

// Usage
const systemPrompt = `
You are an expert portfolio content reviewer.
- You're critical but constructive
- You focus on SEO and user engagement
- You provide actionable feedback
- You maintain professional tone
- You output only JSON unless requested otherwise
`;

const userPrompt = "Review this project description...";
const feedback = await callClaudeWithSystem(userPrompt, systemPrompt);
```

### 3. Token Streaming (Real-time Responses)

```javascript
// Get responses as they're generated (for long tasks)
async function streamClaude(prompt) {
  const stream = await client.messages.stream({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [{ role: "user", content: prompt }]
  });

  for await (const event of stream) {
    if (event.type === 'content_block_delta') {
      process.stdout.write(event.delta.text);
    }
  }
}
```

---

## Monitoring & Logging

### Log All Claude Calls

```javascript
// Create audit trail
function logClaudeCall(prompt, response, cost) {
  const log = {
    timestamp: new Date().toISOString(),
    model: "claude-3-5-sonnet-20241022",
    prompt_length: prompt.length,
    prompt_tokens: response.usage.input_tokens,
    output_tokens: response.usage.output_tokens,
    cost: cost,
    success: true
  };

  // Append to log file
  const fs = require("fs");
  const logPath = "claude-calls.jsonl";  // JSONL format (one JSON per line)
  fs.appendFileSync(logPath, JSON.stringify(log) + "\n");

  return log;
}

// View logs
function getClaudeLogs(filter = {}) {
  const fs = require("fs");
  const logs = fs.readFileSync("claude-calls.jsonl", "utf-8")
    .split("\n")
    .filter(line => line)
    .map(JSON.parse);

  // Filter by date, model, etc.
  return logs;
}
```

---

## Testing Claude Integration

### Unit Tests

```javascript
// tests/claude.test.js
const assert = require("assert");
const { validateProjectData } = require("../agents/claude");

describe("Claude Integration", () => {
  it("should validate correct project data", async () => {
    const validProject = {
      id: "proj_123",
      title: "Test Project",
      description: "A test project...",
      tags: ["Design", "Branding"]
    };

    const result = await validateProjectData(validProject);
    assert.strictEqual(result.valid, true);
  });

  it("should catch missing required fields", async () => {
    const invalidProject = {
      title: "Incomplete Project"
      // Missing required fields
    };

    const result = await validateProjectData(invalidProject);
    assert.strictEqual(result.valid, false);
    assert(result.errors.length > 0);
  });
});
```

### Local Testing (Without API Cost)

```bash
# Test prompt without calling API
node scripts/validateWithClaude.js --dry-run

# Mock Claude responses for testing
export CLAUDE_MODE=mock
npm test
```

---

## Best Practices

### ✅ DO

- ✅ Use structured prompts with clear format requirements
- ✅ Ask Claude to output JSON for programmatic use
- ✅ Set appropriate max_tokens based on expected response
- ✅ Use system messages to define behavior
- ✅ Log all calls for auditing and cost tracking
- ✅ Implement retry logic for transient failures
- ✅ Validate Claude's output before using it
- ✅ Test with dry-run before deploying agents

### ❌ DON'T

- ❌ Send sensitive user data to Claude (email addresses, etc.)
- ❌ Use overly long prompts (keep under 5000 tokens)
- ❌ Make sequential API calls that could be batched
- ❌ Trust Claude output without validation
- ❌ Forget to set max_tokens (can lead to unexpected costs)
- ❌ Use production API key in local testing
- ❌ Deploy agents without human review gates

---

## Quick Reference

### Models to Use

```javascript
// For most tasks (fastest, cheapest)
"claude-3-5-sonnet-20241022"

// For complex reasoning (slower, more expensive)
"claude-3-opus-20250219"

// For simple tasks (super fast, very cheap)
"claude-3-haiku-20250307"
```

### Common Response Format

```json
{
  "valid": true,
  "errors": [],
  "warnings": [],
  "suggestions": [],
  "data": {},
  "score": 85
}
```

### Typical Prompt Structure

```
[ROLE]: You are a...
[CONTEXT]: Background information...
[TASK]: Specific task...
[CONSTRAINTS]: Limitations...
[FORMAT]: Expected output format (JSON, Markdown, etc.)
[EXAMPLES]: Optional examples
```

---

## Resources

- **Claude API Docs:** https://docs.anthropic.com
- **SDK GitHub:** https://github.com/anthropics/sdk-python
- **Prompt Engineering Guide:** https://docs.anthropic.com/guides/prompt-engineering
- **API Status:** https://status.anthropic.com

---

## Support & Help

### Troubleshooting

| Issue | Solution |
|---|---|
| API key not working | Verify in console.anthropic.com, check GitHub secrets |
| Responses too short | Increase `max_tokens` parameter |
| Expensive calls | Use cheaper model or reduce token limits |
| JSON parsing fails | Add `"respond only with JSON"` to prompt |
| Rate limited | Implement exponential backoff retry |

### Community & Resources

- **GitHub Discussions:** github.com/anthropics/sdk-js/discussions
- **Discord:** Join Anthropic community server
- **Email Support:** support@anthropic.com (paid plans)

---

## Next Steps

1. **Get API Key:** https://console.anthropic.com
2. **Store in Secrets:** GitHub repo settings
3. **Test Locally:** Run example scripts
4. **Deploy Agent:** Create GitHub Action workflow
5. **Monitor Costs:** Track API usage monthly
6. **Iterate:** Improve prompts based on results

---

*Last Updated: September 2026*
*Claude Integration Guide v1.0*
