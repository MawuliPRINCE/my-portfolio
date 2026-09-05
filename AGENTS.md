# AGENTS.md
## AI Agent Integration & Automation Guide

**Document Version:** 1.0  
**Last Updated:** September 2026  
**Purpose:** Guidelines for integrating AI agents and automation into the portfolio system

---

## Overview

This document outlines strategies for integrating AI agents (like Claude, GPT-4, or custom bots) with the portfolio website to automate workflows, enhance content management, and improve user interactions.

---

## Current System Readiness

### ✅ Ready for Agent Integration
- JSON-based content structure (easily parseable)
- GitHub API integration (agent can fetch/push content)
- Webhook support potential (GitHub Actions)
- Static site architecture (easy to test changes)
- Version-controlled everything (easy rollback)

### ⚠️ Future Enhancements Needed
- Backend API for real-time data processing
- Database for storing agent decisions/logs
- Authentication system for agent access
- Rate limiting and security policies

---

## Agent Use Cases

### 1. Content Management Agent

**Purpose:** Automate portfolio updates and content organization

#### 1.1 Project Data Validation
```javascript
// Agent Task: Validate new project entries in projects.json
Agent Input: {
  projectData: {
    "id": "proj_1234567890",
    "title": "New Project Title",
    "tags": ["Design", "Branding"],
    "description": "Case study text...",
    "media": [{ "url": "...", "type": "image" }]
  }
}

Agent Validation Rules:
✓ ID is unique (not duplicated)
✓ Title is 3-60 characters
✓ Tags are from approved list
✓ Description includes case study structure
✓ Media URLs are valid Cloudinary links
✓ Required fields present

Agent Output: {
  "valid": true,
  "errors": [],
  "suggestions": ["Add client testimonial", "Include timeline"],
  "correctedData": { /* improved version */ }
}
```

#### 1.2 SEO Metadata Generation
```javascript
// Agent Task: Auto-generate SEO titles & descriptions
Agent Input: {
  project: {
    "title": "MIKITA Fine Jewelry Brand Identity",
    "description": "Full case study HTML..."
  }
}

Agent Tasks:
1. Extract key information from project description
2. Generate compelling SEO title (50-60 chars)
3. Create meta description (150-160 chars)
4. Suggest relevant OG image from media

Agent Output: {
  "seoTitle": "MIKITA Jewelry Brand Identity Design | Prince",
  "seoDescription": "Luxury jewelry brand identity system with elegant logo design, packaging, and visual identity...",
  "seoImage": "https://cloudinary.com/.../mikita-cover.jpg"
}
```

#### 1.3 Content Gap Detection
```javascript
// Agent Task: Identify missing content or inconsistencies
Agent Input: {
  site: {
    projects: [ /* all projects */ ],
    about: { /* about data */ },
    home: { /* home data */ }
  }
}

Agent Analysis:
- Projects without case study descriptions
- Missing media for featured projects
- Outdated project dates
- Skills listed but no project examples
- Social links with no verified profiles

Agent Output: {
  "gaps": [
    { "type": "missing_testimonial", "project": "CHOP BOX" },
    { "type": "outdated_skill", "skill": "3D Design" },
    { "type": "broken_social", "platform": "Instagram" }
  ],
  "recommendations": [ /* fixes */ ]
}
```

---

### 2. Social Media Agent

**Purpose:** Auto-generate social media content from portfolio

#### 2.1 Social Post Generator
```javascript
// Agent Task: Generate Instagram caption for new project
Agent Input: {
  project: {
    "title": "DRIPR Product Packaging",
    "description": "Modern beverage design...",
    "tags": ["Packaging", "Beverage", "Minimalist"],
    "media": [{ "url": "...", "type": "image" }]
  },
  platform: "instagram"
}

Agent Generation:
✓ Emoji selection appropriate to project
✓ Hashtag suggestions (10-15 relevant hashtags)
✓ Caption tone matches brand voice
✓ CTA (link to portfolio project)
✓ Caption length optimized (2200 chars max)

Agent Output: {
  "caption": "🍶 New project alert! DRIPR is a modern beverage design...",
  "hashtags": "#packaging #design #beverage #minimalist #productdesign...",
  "ctaLink": "mawuliprince.github.io/my-portfolio/",
  "imageAlt": "DRIPR beverage packaging design"
}
```

#### 2.2 Content Calendar Planning
```javascript
// Agent Task: Generate monthly social content calendar
Agent Input: {
  projects: [ /* featured projects */ ],
  upcomingEvents: [ /* client launches, milestones */ ],
  platform: "instagram"
}

Agent Planning:
- Space out project features (1 per week)
- Mix project posts with behind-the-scenes content
- Align with industry events/trends
- Balance visual diversity

Agent Output: {
  "contentCalendar": [
    {
      "date": "2026-09-10",
      "type": "project_feature",
      "project": "MIKITA",
      "content": { /* generated caption */ }
    },
    {
      "date": "2026-09-17",
      "type": "behind_the_scenes",
      "content": "Design process video for DRIPR..."
    }
  ]
}
```

---

### 3. Client Communication Agent

**Purpose:** Automate inquiry responses and scheduling

#### 3.1 Inquiry Triage Bot
```javascript
// Agent Task: Analyze incoming inquiries and categorize
Agent Input: {
  inquiry: {
    "name": "John Doe",
    "email": "john@company.com",
    "message": "Hi! We need branding for our startup. Budget ~$5k. Timeline: 3 months.",
    "timestamp": "2026-09-05T10:30:00Z"
  }
}

Agent Analysis:
- Classify inquiry type (branding, packaging, logo, etc.)
- Extract budget & timeline
- Assess fit with designer's services
- Urgency level (high/medium/low)
- Recommended response template

Agent Output: {
  "type": "brand_identity",
  "budget": "$5000",
  "timeline": "3 months",
  "fit_score": 8.5,  // 0-10
  "urgency": "medium",
  "recommended_template": "startup_branding_response",
  "suggested_questions": [
    "What industry/market?",
    "Do you have visual references?",
    "Who's the decision maker?"
  ]
}
```

#### 3.2 Response Generator
```javascript
// Agent Task: Generate personalized response email
Agent Input: {
  inquiry: { /* from above */ },
  designer_context: {
    "recent_work": "CHOP BOX, Terrayield Farms",
    "specialties": "Food & Beverage, Agriculture",
    "availability": "3 slots available this quarter"
  }
}

Agent Generation:
- Personalized greeting with name
- Acknowledge their specific needs
- Highlight relevant past work
- Set clear next steps
- Professional but warm tone

Agent Output: {
  "email": {
    "subject": "Let's Bring Your Startup Brand to Life",
    "body": "Hi John,\n\nThanks for reaching out!...",
    "cta": "Let's schedule a 15-min discovery call",
    "signature": "Best regards,\nIsmael"
  },
  "schedule_link": "calendly.com/ismael/discovery"
}
```

---

### 4. Analytics & Insights Agent

**Purpose:** Process analytics data and generate insights

#### 4.1 Performance Report Generator
```javascript
// Agent Task: Analyze Google Analytics and create report
Agent Input: {
  analytics: {
    "period": "last_30_days",
    "data": {
      "sessions": 1250,
      "users": 980,
      "pageviews": 3400,
      "bounce_rate": 42,
      "avg_session": "2m 15s",
      "top_pages": {
        "projects": 1200,
        "about": 450,
        "home": 1750
      }
    }
  }
}

Agent Analysis:
- Calculate trends (up/down vs previous period)
- Identify top-performing pages
- Flag underperforming content
- Detect traffic source patterns
- Generate actionable recommendations

Agent Output: {
  "summary": "Strong month with 15% user growth...",
  "highlights": [
    "Projects page getting 35% more traffic",
    "Mobile traffic up 22%",
    "Dribbble referral driving 120 visits"
  ],
  "opportunities": [
    "About page engagement low (13%)",
    "No contact CTA clickthroughs tracked",
    "Blog content could boost SEO"
  ],
  "recommendations": [
    "Add video to projects page",
    "Improve about page CTAs",
    "Start publishing design insights"
  ]
}
```

#### 4.2 Trend Detection Agent
```javascript
// Agent Task: Identify emerging trends in portfolio performance
Agent Input: {
  analytics_history: [
    { date: "2026-08", projects_views: 1050 },
    { date: "2026-07", projects_views: 950 },
    { date: "2026-06", projects_views: 880 }
  ],
  projects: [ /* all projects */ ]
}

Agent Tasks:
- Correlate project popularity with specific tags
- Track which design categories attract most interest
- Monitor traffic source efficiency
- Predict future demand

Agent Output: {
  "trending": [
    { "tag": "Branding", "trend": "up 18%", "recommendation": "Feature more branding work" },
    { "tag": "Packaging", "trend": "up 12%", "recommendation": "Pursue packaging clients" },
    { "tag": "Product Design", "trend": "down 5%", "recommendation": "Showcase product design progress" }
  ],
  "prediction": "Branding inquiries likely to increase 20-30% next quarter"
}
```

---

### 5. Content Improvement Agent

**Purpose:** Enhance existing content quality

#### 5.1 Copy Optimization
```javascript
// Agent Task: Improve project descriptions for clarity & SEO
Agent Input: {
  project: {
    "title": "CHOP BOX",
    "description": "Brand identity and packaging design for a streetfood restaurant..."
  }
}

Agent Improvements:
- Add structural headings (Challenge, Solution, Result)
- Incorporate relevant keywords naturally
- Expand on design rationale
- Add quantifiable results/metrics
- Ensure readability (shorter sentences)

Agent Output: {
  "improved_description": "<h3>Challenge</h3><p>CHOP BOX needed...</p>...",
  "seo_keywords": ["streetfood branding", "restaurant design", "packaging"],
  "readability_score": 9.2,  // 0-10
  "estimated_read_time": "3 minutes"
}
```

#### 5.2 Image Alt Text Generator
```javascript
// Agent Task: Generate descriptive alt text for images
Agent Input: {
  project: "MIKITA",
  image: {
    "url": "https://cloudinary.com/.../jewelry-logo.png",
    "context": "Logo design for luxury jewelry brand"
  }
}

Agent Generation:
- Describe image content accurately
- Include relevant keywords
- Accessible for screen readers
- Concise but descriptive (125 chars max)

Agent Output: {
  "altText": "MIKITA luxury jewelry brand logo featuring elegant serif letterforms and minimalist gold design"
}
```

---

## Agent Architecture

### Agent Workflow Pipeline

```
┌─────────────────────────────────────────────────────────┐
│              Agent Trigger Event                        │
│     (GitHub Push, Schedule, Manual, Webhook)           │
└──────────────────────┬──────────────────────────────────┘
                       │
        ┌──────────────▼──────────────┐
        │  Agent Initialization       │
        │  - Load context             │
        │  - Retrieve CMS data        │
        │  - Set task parameters      │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │  AI Processing              │
        │  - Claude API call          │
        │  - Process instructions     │
        │  - Generate/analyze output  │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │  Output Validation          │
        │  - Check quality            │
        │  - Verify correctness       │
        │  - Human review (optional)  │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │  Implementation             │
        │  - Update JSON files        │
        │  - Commit to GitHub         │
        │  - Trigger site rebuild     │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │  Logging & Monitoring       │
        │  - Track success/failure    │
        │  - Alert on issues          │
        │  - Log for audit trail      │
        └──────────────────────────────┘
```

---

## Implementation Guide

### Step 1: Set Up Agent Credentials

```bash
# Store API keys securely in GitHub Secrets
# Settings → Secrets and variables → Actions

CLAUDE_API_KEY=sk-...
GITHUB_TOKEN=ghp_...
```

### Step 2: Create Agent Function

```javascript
// agents/contentValidator.js
const Anthropic = require("@anthropic-ai/sdk");

class ContentValidationAgent {
  constructor(apiKey) {
    this.client = new Anthropic({ apiKey });
  }

  async validateProjectData(projectData) {
    const message = await this.client.messages.create({
      model: "claude-3-5-sonnet-20241022",
      max_tokens: 1024,
      messages: [
        {
          role: "user",
          content: `Validate this project data and provide feedback:\n${JSON.stringify(projectData, null, 2)}`
        }
      ]
    });
    
    return message.content[0].text;
  }
}

module.exports = ContentValidationAgent;
```

### Step 3: Set Up GitHub Action Workflow

```yaml
# .github/workflows/agent-content-validator.yml
name: Content Validation Agent

on:
  push:
    paths:
      - 'projects.json'
  schedule:
    - cron: '0 9 * * MON'  # Weekly Monday check

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Content Validation Agent
        env:
          CLAUDE_API_KEY: ${{ secrets.CLAUDE_API_KEY }}
        run: |
          node agents/contentValidator.js
      
      - name: Auto-commit improvements
        if: ${{ success() }}
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "Agent: Auto-validated content"
```

---

## Safety & Guardrails

### 1. Human Review Gates

```javascript
// Require human approval for major changes
const REQUIRES_APPROVAL = [
  'delete_project',      // Removing portfolio work
  'update_hero_image',   // Changing home page
  'modify_bio',          // Changing about section
  'new_social_platform'  // Adding new contact method
];

// Non-critical changes auto-approved
const AUTO_APPROVED = [
  'fix_typo',           // Grammar/spelling
  'add_alt_text',       // Accessibility
  'update_tags',        // Categorization
  'generate_seo_meta'   // SEO metadata
];
```

### 2. Rollback Capability

```bash
# Easy rollback if agent makes mistakes
git revert <commit-hash>
git push

# GitHub Pages auto-redeploys previous version
```

### 3. Rate Limiting

```javascript
// Prevent runaway agent costs
const RATE_LIMITS = {
  claude_api_calls_per_day: 100,
  max_tokens_per_call: 2048,
  cost_budget_monthly: 50  // Stop if exceeding budget
};
```

### 4. Audit Trail

```javascript
// Log all agent actions for review
const auditLog = {
  timestamp: new Date().toISOString(),
  agent: "ContentValidationAgent",
  action: "validate_project",
  input: projectData,
  output: validationResult,
  status: "success",
  cost: "$0.02"
};

// Save to database or CSV for auditing
```

---

## Agent Prompting Best Practices

### Effective Prompts

```markdown
# ✅ GOOD: Specific, structured, clear
Task: Analyze the portfolio project data and:
1. Check all required fields are present
2. Validate data types match schema
3. Suggest improvements for clarity
4. Flag any potential issues

Constraints:
- Project title must be 3-60 characters
- Year must be 4-digit number
- Tags must be from approved list
- Description must include case study structure

Format output as JSON with:
{
  "valid": boolean,
  "errors": [],
  "suggestions": [],
  "correctedData": {}
}

# ❌ BAD: Vague, unstructured
Task: Make the projects better

Format: Whatever works
```

### Context Window Management

```javascript
// Provide necessary context without bloat
const prompt = `
You are a content validation agent for a design portfolio.

[CONTEXT]
- Portfolio owner: Ismael Asumanu Prince
- Specialties: Branding, Packaging, Visual Identity
- Tone: Professional, creative, intentional
- Target audience: Potential clients, design industry peers

[TASK]
Validate the following project data...

[VALIDATION RULES]
1. ...
2. ...
3. ...

[FORMAT]
Output as JSON...
`;

// Keep prompts under ~3000 tokens for cost efficiency
```

---

## Testing & Development

### Local Testing

```bash
# Test agent without GitHub Actions
node agents/contentValidator.js --test --dry-run

# Output: Shows what changes would be made (no actual changes)
```

### Staging Environment

```bash
# Test agent on staging branch before deploying to main
git checkout -b agent/testing
# Run agent with TEST_MODE=true
# Review changes, then merge to main
```

---

## Monitoring & Alerts

### Success Metrics

```
Agent Task Completion Rate:  Target: 95%+
Average Processing Time:     Target: < 30 seconds
Error Rate:                  Target: < 5%
Cost Per Task:              Target: < $0.05
User Approval Rate:         Target: 90%+
```

### Alert Triggers

```javascript
const ALERT_CONDITIONS = {
  failed_task: "Email notify on task failure",
  high_cost: "Notify if daily spend > $10",
  approval_pending: "Reminder after 24hrs pending review",
  rate_limit_hit: "Alert when hitting API limits",
  low_accuracy: "Flag if validation accuracy drops"
};
```

---

## Future Agent Capabilities

### Phase 2
- [ ] Automated project categorization
- [ ] Client inquiry auto-routing
- [ ] Smart content recommendations
- [ ] Sentiment analysis of feedback
- [ ] Competitor design analysis

### Phase 3
- [ ] Generative portfolio variations (A/B testing)
- [ ] Predictive client needs
- [ ] AI-powered design process documentation
- [ ] Automated client onboarding
- [ ] Custom report generation

---

## Troubleshooting Agents

| Issue | Cause | Solution |
|---|---|---|
| Agent produces poor output | Vague prompt | Refine prompt with examples |
| High costs | Too many API calls | Add caching, reduce frequency |
| Changes not committing | GitHub token expired | Refresh GITHUB_TOKEN secret |
| Slow processing | Large data files | Split into batches |
| Incorrect validations | Wrong schema | Update validation rules |

---

## Resources

- **Claude API Docs:** https://claude.ai/docs
- **GitHub Actions:** https://docs.github.com/en/actions
- **Agent Framework:** Anthropic Python/JavaScript SDK
- **Example Agents:** See `/agents` directory in repo

---

## Contact & Support

For questions about agent implementation:
- Review CLAUDE.md for Claude-specific integration
- Check ARCHITECTURE.md for system design
- Refer to example agents in repo

---

*Last Updated: September 2026*
*Agent Integration Guide v1.0*
