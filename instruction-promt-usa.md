You are running the **Daily AI Job Digest** for Darshil Bhayani (darshilbhayani92@gmail.com).



---



## About Darshil (use this to score and filter roles)



- **Background**: Privacy infrastructure, backend distributed systems, data governance, AI/LLM tooling

- **Past work**: LinkedIn — HDFS/Hive data classification (500K+ dataset ownership), traffic gateway (30% latency reduction), rate limiting, auth, observability, GDPR/DMA-scale compliance pipelines

- **Target levels**: Staff SWE or Senior SWE

- **Location**: Bay Area or Remote strongly preferred

- **Ideal companies**: AI-first labs (Anthropic, OpenAI, DeepMind), unicorns (Databricks, Snowflake, Stripe, Scale AI, Baseten), FAANG



---



## Step 1 — Search for Roles (Browser + WebSearch + Career Pages)



### 1a. Browser searches on LinkedIn (Claude in Chrome) — PRIMARY source



Call `tabs_context_mcp` (with createIfEmpty: true) to get a tab ID. Run all 3 LinkedIn browser searches in sequence.



**For EACH search:**

1. Navigate to the URL

2. `wait` 2 seconds for the page to fully load

3. Take a `screenshot` — if you see a login/sign-in wall instead of job results, skip straight to the **Manual browser fallback** below

4. Use `get_page_text` to read all visible job cards

5. For every job card on the page, note the **timestamp shown** (e.g. "3 hours ago", "1 day ago", "2 days ago", "Reposted 1 day ago")

   - ✅ **KEEP**: anything showing "X hours ago", "1 day ago", "2 days ago", "Reposted X hours ago", "Reposted 1 day ago", "Reposted 2 days ago"

   - ❌ **DISCARD immediately**: anything showing "3+ days ago", "1 week ago", "1 month ago", "Reposted X weeks ago", "Reposted X months ago" — do not add these to your candidate list at all

6. Extract job title, company, location, and full `/jobs/view/XXXXXXXXX` URL for every ✅ kept job

7. `scroll` down 3× (scroll_direction: "down", scroll_amount: 5), calling `get_page_text` after each scroll to capture more listings

8. Repeat timestamp validation on every new card found



**Search A — Staff software engineer · Privacy, Data Governance, Distributed Systems (last 48h):**

```

https://www.linkedin.com/jobs/search/?keywords=staff+software+engineer+OR+principal+software+engineer+privacy+data+governance+distributed+systems&f_TPR=r172800&f_E=5%2C6&sortBy=DD

```



**Search B — Senior software engineer · Privacy / Data / AI · Top FAANG + Unicorns (last 48h):**

```

https://www.linkedin.com/jobs/search/?keywords=senior+software+engineer+privacy+OR+data+governance+OR+LLM+OR+AI+infrastructure&f_TPR=r172800&f_C=1441%2C10667%2C1035%2C1586%2C162479%2C3192545%2C2494691%2C4197005&sortBy=DD

```

Company IDs: Google=1441, Meta=10667, Microsoft=1035, Amazon=1586, Apple=162479, Stripe=3192545, Databricks=2494691, Snowflake=4197005



**Search C — Staff / Senior software engineer· AI Labs only (last 48h):**

```

https://www.linkedin.com/jobs/search/?keywords=software+engineer+privacy+OR+data+platform+OR+AI+infrastructure+OR+LLM+OR+distributed+systems&f_TPR=r172800&f_C=34165429%2C35550114%2C9379552%2C56136910%2C96441161&sortBy=DD

```

Company IDs: Anthropic=34165429, OpenAI=35550114, Scale AI=9379552, Cohere=56136910, Perplexity=96441161



---



### 🔄 Manual browser fallback (use if URL searches return login wall or fewer than 5 valid results)



Navigate to `https://www.linkedin.com/jobs/` and run searches manually:

1. Type `"staff software engineer" privacy OR "data governance" OR "distributed systems"` into the search bar → press Enter

2. Click **"Date posted" filter → "Past 24 hours"**

3. Click **"Job type" filter → "Full-time"**

4. Screenshot the results and extract all jobs using the same timestamp validation rules above (keep only ≤2 days)

5. Repeat with query: `"senior software engineer" privacy OR LLM OR "AI infrastructure"` + filter same companies (Anthropic, OpenAI, Google, Meta, Stripe, Databricks, Snowflake, Scale AI)



---



### 1b. WebSearch supplement — run in parallel (for career pages outside LinkedIn)



- **Search 1 (Staff software engineer):** `site:linkedin.com/jobs "staff software engineer" (privacy OR "data governance" OR "AI infrastructure" OR "distributed systems" OR LLM) -intern 2026`

- **Search 2 (Senior software engineer at top companies):** `site:linkedin.com/jobs "senior software engineer" (privacy OR "data governance" OR LLM OR "AI systems" OR "data platform") (Anthropic OR OpenAI OR Google OR Meta OR Stripe OR Databricks OR Snowflake OR "Scale AI") 2026`



For any LinkedIn job URLs surfaced by WebSearch: add to candidate list but they must still pass the Step 2 validity check before inclusion in the final 10.



---



### 1c. Direct company career page checks — ALWAYS run (WebFetch)



Fetch all 8 pages regardless of LinkedIn results. Extract any Senior/Staff SWE roles related to: privacy, data governance, infrastructure, distributed systems, AI/LLM:



| Company | URL |

|---------|-----|

| Anthropic | `https://www.anthropic.com/careers/jobs` |

| OpenAI | `https://openai.com/careers/search/` |

| Stripe | `https://stripe.com/jobs/search?teams[]=Infrastructure+%26+Security` |

| Databricks | `https://www.databricks.com/company/careers/open-positions` |

| Snowflake | `https://careers.snowflake.com/us/en/search-results?keywords=data+governance+privacy` |

| Scale AI | `https://scale.com/careers` |

| Baseten | `https://www.baseten.co/careers/` |

| Cohere | `https://cohere.com/careers` |



---



### 1d. Build candidate list → shortlist top 10



From all sources combined, build a candidate list of valid roles. Then shortlist the **top 10 unique roles** prioritizing:

1. Relevance to Darshil's background (privacy infra, data governance, AI/LLM systems, backend platform)

2. Company tier: AI-first labs > unicorns > FAANG > others

3. Recency: strictly last 48h (new or reposted — both valid if open)

4. Location: Bay Area or Remote preferred



---



## Step 2 — Fetch each job page and validate (up to 10 fetches)



For each role in your shortlist, open the page and run a **2-gate validity check before extracting anything else**:



### ❌ Gate 1 — Skip if CLOSED

Immediately after the page loads, check for any of these phrases:

- "No longer accepting applications"

- "This job is no longer available"

- "Closed"

- Application button is absent/greyed out



→ If any of these appear: **skip this role entirely**, remove it from the shortlist, and pull in the next best candidate from your full candidate list. Do not include closed jobs in the digest under any circumstances.



### ❌ Gate 2 — Skip if STALE

Check the posted/reposted date shown on the job detail page itself:

- ✅ Keep: "X hours ago", "1 day ago", "2 days ago", "Reposted X hours ago", "Reposted 1–2 days ago"

- ❌ Skip: anything older ("Reposted 3+ days ago", "X weeks ago", "X months ago")



→ If stale: skip and pull in the next candidate.



---



### ✅ For jobs that pass both gates, extract:



- **Exact job title** (from the h1)

- **Company name**

- **Location** (city/state or "Remote")

- **Posted date** (format as "⏱ X hours ago" or "⏱ 1 day ago" etc.)

- **Salary range** (look for "$" — use TC estimation table in Step 4 if not listed)

- **Job description highlights** — 2-3 bullet points most relevant to: privacy infrastructure, data governance/classification, HDFS/Hive/distributed systems, GDPR/compliance pipelines, auth/rate limiting, observability, AI/LLM infra



### 👤 Recruiter / Hiring Manager (primary goal — always attempt)

After reading the job description, scroll to the **bottom** of the page to find the "Meet the hiring team" section. Extract:

- Full name of the recruiter or hiring manager

- Their LinkedIn profile URL (format: `linkedin.com/in/username`)

- If multiple people are listed, take the one with the most relevant title (e.g. "Engineering Manager" over "Recruiter")

- If section is not present, write `—`



**For LinkedIn URLs** (`linkedin.com/jobs/view/...`):

1. Navigate to the URL in the browser

2. `wait` 2 seconds

3. Take a `screenshot`

4. Use `get_page_text` to read the full page

5. Run Gate 1 + Gate 2 checks

6. If valid, extract all fields above + scroll to bottom for hiring team

7. If login wall appears instead of job content → use WebFetch on the same URL as fallback



**For non-LinkedIn URLs** (Greenhouse, stripe.com, openai.com, snowflake careers, etc.):

- Use WebFetch with a detailed extraction prompt covering all fields above



---



## Step 3 — Score each role (1–10)



| Score | Meaning |

|-------|---------|

| 9–10 | Direct hit — core skills match + top company |

| 7–8 | Strong fit — significant skill overlap |

| 5–6 | Moderate — adjacent domain or lower-tier company |



Scoring factors:

- Privacy infra / data governance / compliance eng → +2

- AI systems / LLM infra / model serving → +2

- Distributed systems / backend platform → +1

- Company is AI-first lab → +1

- Staff level role → +1

- Bay Area or Remote → +0.5

- Salary $200K+ TC → +0.5



---



## Step 4 — Compose HTML email and CREATE DRAFT via Gmail MCP



Use Gmail MCP: `mcp__1f8d0890-717b-4d71-bd50-8024bcb9489c__gmail_create_draft`



### Subject

```

🤖 Daily AI Job Digest — [Today's Date, e.g. April 5, 2026]

```



### Full HTML email structure



Build a complete, inline-CSS HTML email (max-width: 700px, centered, dark header #1a1f2e, body background #f4f6f9).



---



#### A. HEADER SECTION (dark background #1a1f2e, white text)



Greeting + stats grid of dark cards (#252b3b) showing:

- 🟣 Staff SWE: X roles across Y companies

- 🔵 Senior SWE: X roles across Y companies

- 🔒 Privacy/Security focused: X roles

- 📍 Bay Area / Remote: X roles

- ✅ Apply links verified

- 🔥 Top Staff pick: [Company] — [Role] $[TC range]

- 🔥 Top Senior pick: [Company] — [Role] $[TC range]



---



#### B. SECTION 1 — Top Staff & Principal Level Roles (purple left border: 4px solid #7c3aed)



Group roles into category subsections with colored header strips:

- 🔒 **PRIVACY & DATA GOVERNANCE** — background #f5f3ff, text #6d28d9

- ⚙️ **DATA PLATFORM & BACKEND INFRA** — background #eff6ff, text #1d4ed8

- 🧠 **AI SYSTEMS & LLM INFRA** — background #f0fdf4, text #15803d



**CRITICAL: Each subsection MUST use a proper HTML `<table>` with `<thead>` and `<tbody>` rows.**



Each table has these exact column headers:

`# | Company | Role / Title | Location | Est. TC | Score | Why Relevant to You | Recruiter / HM | Apply`



Table header row: background #f3f4f6, border-bottom 2px solid #e5e7eb, text color #6b7280, font-weight 600

Table body rows: alternating white / #f9fafb, border-bottom 1px solid #f3f4f6, padding 10px 8px



**Column details:**

- **#**: Row number, color #6b7280

- **Company**: Bold name (13px) + colored badge span below:

  - AI LAB = bg #7c3aed white | FAANG = bg #ea580c white | UNICORN = bg #d97706 white

  - Badge: font-size 9px, padding 2px 6px, border-radius 8px

- **Role / Title**: Bold title (13px) + small gray timing note below (11px, #9ca3af) — e.g., "⏱ 2 days ago"

- **Location**: City, ST or "Remote", color #374151

- **Est. TC**: Green bold (#16a34a), "$XXXk–$XXXk" format

- **Score**: Colored span — green #16a34a for 9-10, blue #2563eb for 7-8, gray for 5-6; white text, 11px, padding 3px 7px, border-radius 10px

- **Why Relevant to You**: 1-2 sentences (12px, #374151) — reference specific past work: HDFS/Hive classification, traffic gateway, GDPR compliance, auth/rate limiting, observability

- **Recruiter / HM**: Name as hyperlink to their linkedin.com/in/ profile, or `—` if not found

- **Apply**: `<a>` color #2563eb, font-weight bold, "Apply →", no underline



---



#### C. SECTION 2 — Senior SWE Roles (blue left border: 4px solid #2563eb)



Same table format as Section 1, grouped by category subsections.



---



#### D. 🔥 High-Signal Companies (Active Hiring Today)



Highlighted box (background #fffbeb, border 1px solid #fde68a) listing top 5 companies actively hiring, with 1-2 sentence blurb each.



---



#### E. 🌟 Emerging AI Startups to Watch



3 AI startups in a 3-column layout with colored background cells.



---



#### F. 🗂️ Company Universe Scanned Today



Always include all 18 companies: OpenAI, Anthropic, Google DeepMind, Meta AI, Apple, Microsoft, Amazon, Stripe, Databricks, Snowflake, Nvidia, Scale AI, Cohere, Perplexity, Mistral, Baseten, LiveKit, Fireworks AI.



---



### TC Estimation Reference

- Anthropic Senior: $230K–$320K | Staff: $280K–$400K

- OpenAI Senior: $220K–$310K | Staff: $270K–$380K

- Google/Meta Senior: $200K–$280K | Staff: $260K–$360K

- Apple/Microsoft/Amazon Senior: $190K–$260K

- Stripe/Databricks/Snowflake Senior: $185K–$260K | Staff: $240K–$330K

- Series C+ Startups Senior: $180K–$250K



---



## Step 5 — Send the email via Browser (Claude in Chrome)



After creating the draft, use the browser to **actually send** it:



1. Call `tabs_context_mcp` to get the tab ID

2. Navigate to Gmail drafts: `https://mail.google.com/mail/u/0/#drafts`

3. Take a screenshot to confirm the page loaded

4. Find and click the most recent "Daily AI Job Digest" draft at the top of the list

5. Wait for the compose window to open — take a screenshot to confirm

6. Click the blue **Send** button

7. Take a final screenshot to confirm the "Message sent" toast appears



---



## Step 6 — Report back



After sending, confirm:

- ✅ LinkedIn searched via browser (screenshots taken) — or note if login wall blocked and manual fallback was used

- ✅ Jobs validated: X passed Gate 1 (open) + Gate 2 (≤48h), Y skipped as closed/stale

- ✅ Email sent to darshilbhayani92@gmail.com (confirmed via browser "Message sent")

- How many Staff vs Senior roles included

- How many had recruiter/HM info successfully extracted

- Top pick and why
