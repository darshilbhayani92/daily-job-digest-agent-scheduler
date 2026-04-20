---
name: senior-swe-job-search
description: |
  Find top Senior Software Engineer jobs at leading AI companies, unicorn startups, and tech giants.
  Use this skill when the user asks to search for Senior SWE/Engineer roles at high-growth or top-tier companies,
  especially in AI, machine learning, or emerging tech sectors. This skill will search for the top 15 hiring companies,
  extract their job posting links, and send a consolidated email with all results. Trigger on phrases like:
  - "Find me Senior SWE jobs at top companies"
  - "Search for Senior Engineer roles at AI startups"
  - "Get me a list of companies hiring for Senior SWE"
  - "Find Senior Software Engineer positions at unicorns"
  - Even if not explicitly mentioning "Senior SWE", trigger when the user is job hunting and asks about roles at top-tier tech companies.
compatibility: WebSearch, Gmail MCP (for email delivery)
---

# Senior SWE Job Search Agent

This skill finds and aggregates **real, currently open** Senior Software Engineer job listings across top-tier AI companies, emerging AI unicorn startups, and leading tech firms — using only **2 targeted web searches** with zero individual page fetches, keeping token usage minimal.

## Why This Approach Is Token-Efficient

Most company career pages (OpenAI, Anthropic, Google, etc.) use job board platforms like **Greenhouse**, **Lever**, or **Ashby**. These platforms are indexed by search engines with structured job listing URLs. A single search query against these domains returns actual open roles across dozens of companies simultaneously — with direct apply links in the search snippets. No page fetching required.

## Workflow

### Step 1: Understand the User's Needs (skip if already clear)

Default to:
- **Companies**: AI-first companies + emerging AI unicorns + top-tier tech giants
- **Role**: Senior Software Engineer (and equivalents: Senior SWE, Senior Engineer, Staff Engineer)
- **Output**: Email to `darshilbhayani92@gmail.com`
- **Format**: Ranked table with company, role title, and direct job link

Only ask for clarification if the user wants a specific location filter or niche focus.

### Step 2: Run 2 Targeted Web Searches (no more, no less)

**Search 1 — Job aggregator platforms (catches AI/startup companies):**
```
"senior software engineer" site:greenhouse.io OR site:lever.co OR site:ashbyhq.com AI OR "machine learning" OR LLM 2026
```
This hits the job board platforms that power most AI and unicorn startups and returns actual open listings with direct apply links.

**Search 2 — Big Tech companies with proprietary job boards:**
```
"senior software engineer" jobs 2026 site:careers.google.com OR site:metacareers.com OR site:amazon.jobs OR site:microsoft.com/careers OR site:apple.com/jobs OR site:stripe.com/jobs OR site:openai.com/careers OR site:anthropic.com/careers
```

**Token rules — strictly follow these:**
- ✅ Extract company name, job title, and direct URL from search result snippets only
- ✅ Combine results from both searches into one list
- ❌ Do NOT fetch any individual company career page or job posting URL
- ❌ Do NOT run more than 2 searches
- ❌ Do NOT open/read any page to get additional details

### Step 3: Compile Top 15 Results

From combined search results, select the top 15 unique companies, prioritizing:

1. **AI-first companies** (Anthropic, OpenAI, Mistral, Perplexity, Together AI, Scale AI, Cohere, Hugging Face, Character.ai, xAI, etc.)
2. **Emerging AI unicorns** ($1B+ valuation AI/ML startups from 2024-2026: Harvey, Sierra, Poolside, Magic, Cognition, etc.)
3. **Top-tier tech giants** (Google, Meta, Apple, Microsoft, Amazon, Stripe, Databricks, Snowflake, Nvidia, etc.)

Format as a clean table:

```
| # | Company | Role | Direct Apply Link |
|---|---------|------|------------------|
| 1 | Anthropic | Senior Software Engineer | https://boards.greenhouse.io/anthropic/jobs/... |
| 2 | OpenAI | Senior Software Engineer | https://openai.com/careers/... |
...
```

If a real direct job link was not found in search results, use the company careers page URL and add "(search: Senior Software Engineer)" as a note.

### Step 4: Send Email via Gmail MCP

Use the Gmail MCP tool to send results to the user.

- **To**: `darshilbhayani92@gmail.com`
- **Subject**: `Top 15 Senior SWE Jobs - AI + Unicorns + Big Tech | {Today's Date}`
- **Body**:

```
Hi Darshil,

Here are the top 15 companies currently hiring for Senior Software Engineer roles spanning AI-first companies, emerging AI unicorns, and top-tier tech giants.

[INSERT TABLE HERE]

How to use this list:
- Direct job links go straight to the posting - apply now before they close
- For career page links, search "Senior Software Engineer" once you land on the page
- All results are current as of {DATE}

Good luck!

--
Senior SWE Job Search Agent
```

Use: `mcp__1f8d0890-717b-4d71-bd50-8024bcb9489c__gmail_create_draft`

### Step 5: Report Back to User

After sending, confirm:
- Email sent to darshilbhayani92@gmail.com
- How many were direct job posting links vs. career page links
- Any standout roles or companies worth highlighting

## Token Budget

| Action | Count | Notes |
|--------|-------|-------|
| Web searches | 2 | Aggregate platforms + Big Tech |
| Page fetches | 0 | Extract from snippets only |
| Gmail MCP call | 1 | One consolidated email |
| **Total tool calls** | **3** | **Minimal** |

## Implementation Notes

- Use `WebSearch` for both queries - never use `WebFetch` to open URLs
- Use Gmail MCP: `mcp__1f8d0890-717b-4d71-bd50-8024bcb9489c__gmail_create_draft`
- User email: `darshilbhayani92@gmail.com`
- Do not cache results unless user explicitly asks for ongoing tracking

---

**Last Updated:** March 25, 2026
**Model Compatibility:** Claude Sonnet 4.6+
