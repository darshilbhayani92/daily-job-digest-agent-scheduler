---
name: daily-ai-faang-job-digest-california-usa
description: Daily AI & FAANG Job Digest — Staff SWE + Senior SWE, LinkedIn last 48h, Bay Area/Remote-US, recruiter info scraped, styled HTML email to darshilbhayani92@gmail.com
---

You are running the Daily AI & FAANG Job Digest for Darshil Bhayani (darshilbhayani92@gmail.com).

**Darshil's relevance profile:** Privacy infrastructure, GDPR/CCPA/CIPA compliance systems, distributed systems, traffic gateways, Kubernetes, backend platform engineering. Staff/Principal-track ambition at AI/FAANG companies.

---

## 🚀 STARTUP — Detect Run Mode

Run this bash command first:
```
HOUR=$(date "+%H"); echo $HOUR
```

- If HOUR is **08** → this is a **scheduled run** → set `RUN_MODE = "full"` and skip to FULL RUN MODE below.
- Otherwise → this is a **manual run** → use the `AskUserQuestion` tool to ask:

**Question:** "Which mode would you like to run?"
**Choices:**
- 🧪 **Smoke Test** — Quick validation (~3k tokens, ~5 min). 1 LinkedIn search, 1 Gmail email, Greenhouse login check only, max 2 detail page visits. No email sent — results shown in chat only.
- 📋 **Full Run** — Complete job digest (~20k tokens, ~15–20 min). All 3 sources, ranked results, styled HTML email sent to darshilbhayani92@gmail.com.

Set `RUN_MODE = "smoke"` or `RUN_MODE = "full"` based on the answer, then jump to the matching section below.

---

---
# ════════════════════════════════════
# 🧪 SMOKE TEST MODE (RUN_MODE = "smoke")
# ════════════════════════════════════
---

**Token budget: ~3,000 tokens. Hard-stop as soon as any cap is hit.**

| Resource | Smoke Cap |
|---|---|
| LinkedIn searches | 1 (Staff SWE only) |
| Jobs from LinkedIn page | Max 2 |
| Gmail emails to read | Max 1 |
| Greenhouse | Login check + screenshot only — no searches |
| Detail page visits | Max 2 total |
| Description per job | Max 100 chars |

### S1 — LinkedIn (Staff SWE only)

Call `tabs_context_mcp` with `createIfEmpty: true`. Note numeric tabId.

Navigate to:
```
https://www.linkedin.com/jobs/search/?keywords=Staff%20Software%20Engineer&location=San%20Francisco%20Bay%20Area&f_TPR=r604800&f_WT=2%2C1%2C3&sortBy=DD
```

Take screenshot. Run JS to extract up to 2 jobs from target companies (see company list in Full Run section). Do NOT scroll. Do NOT visit detail pages yet.

Record: `S1_STATUS`, `S1_JOBS`

### S1b — Gmail (max 1 email)

Call `gmail_search_messages` with `q: "from:jobs-noreply@linkedin.com newer_than:3d"`, `maxResults: 1`.
Read 1 email with `gmail_read_message`. Extract any job title + company + URL from body.

Record: `S1B_STATUS`, `S1B_JOBS`

### S1c — Greenhouse login check (screenshot only)

Call `switch_browser` → verify "Personal" in name.
Navigate to `https://my.greenhouse.io/jobs?default=true`. Take screenshot.
Record: `GH_STATUS = "✅ Logged in" OR "❌ Not logged in"`

### S2 — Detail pages (max 2)

Visit detail pages for up to 2 jobs from S1+S1b (combined). For each:
- Run recruiter JS (see Full Run STEP 2 for the exact snippet)
- Extract first 100 chars of description

### S3 — Gmail validation (no send)

Call `switch_browser` → verify "Personal".
Navigate to `https://mail.google.com/mail/u/0/`. Check tab title contains `darshilbhayani92@gmail.com`.
Record: `GMAIL_STATUS = "✅ Confirmed" OR "❌ Not accessible"`

### Smoke Test Report

Output a plain-text summary to chat:

```
🧪 SMOKE TEST RESULTS
=====================
S1  LinkedIn Search : [S1_STATUS] | Jobs found: [titles + companies]
S1b Gmail Alerts    : [S1B_STATUS] | Jobs found: [titles + companies]
S1c Greenhouse      : [GH_STATUS]
S2  Detail Pages    : Visited [N] pages | Recruiter: [name or —]
S3  Gmail Send Path : [GMAIL_STATUS]

VERDICT: [✅ All systems go / ⚠️ Issues: list them]
```

**STOP here. Do not create a draft or send an email.**

---

---
# ════════════════════════════════════
# 📋 FULL RUN MODE (RUN_MODE = "full")
# ════════════════════════════════════
---

Today's date is available via Bash: `date "+%B %d, %Y"`. Execute all steps without asking questions.

## ⚠️ GLOBAL SEARCH BUDGET — READ FIRST, ENFORCE THROUGHOUT

| Constraint | Limit |
|---|---|
| Staff SWE jobs fully analysed (LinkedIn + Email) | Max **10** |
| Senior SWE jobs fully analysed (LinkedIn + Email) | Max **10** |
| Greenhouse jobs fully analysed | Max **8** |
| Detail page visits total (all sources) | Max **25** |
| Text extracted per job description | Max **200 chars** |
| LinkedIn search scroll passes per query | Max **2** |
| Gmail alert emails to read | Max **15** |
| Greenhouse search pages per query | Max **2** |
| **Estimated total token budget** | **~20,000 tokens** |

**Token-saving rules:**
- Extract title/company/location/URL from search results page first — do NOT visit detail pages yet
- Only visit detail pages for the **final selected candidates** after all filtering
- Call `get_page_text` **once** per page — never twice on the same page
- Truncate all description snippets to **200 characters maximum**
- Stop collecting as soon as caps are hit

**Remainder rule (important):**
After selecting top candidates for full analysis, keep a **remainder list** of every job that passed the company/title filter but did NOT make the top-10 cut. These go into the "Also Found" section — no detail page visits needed, just the raw title + company + URL already extracted from the search page.

---

## STEP 1 — LinkedIn Job Searches via Chrome MCP

All tabId values must be NUMBERS (integers).

1. Call `tabs_context_mcp` with `createIfEmpty: true`. Note numeric tabId.
2. Navigate to Search A. Sleep 3s. Call `get_page_text` once.
3. **Scroll pass 1:** `const l = document.querySelector('.scaffold-layout__list-container'); if(l) l.scrollTop=4000; 'ok';` Sleep 2s. `get_page_text` once more.
4. **Scroll pass 2 (if fewer than 10 target-company Staff jobs found so far):** `if(l) l.scrollTop=8000; 'ok';` Sleep 2s. `get_page_text` once more.
5. Create second tab. Navigate to Search B. Apply same 2-scroll approach.
6. **Stop** — no further pagination.

**Search A (Staff SWE — last 7 days):**
```
https://www.linkedin.com/jobs/search/?keywords=Staff%20Software%20Engineer&location=San%20Francisco%20Bay%20Area&f_TPR=r604800&f_WT=2%2C1%2C3&sortBy=DD
```

**Search B (Senior SWE — last 7 days):**
```
https://www.linkedin.com/jobs/search/?keywords=Senior%20Software%20Engineer&location=San%20Francisco%20Bay%20Area&f_TPR=r604800&f_WT=2%2C1%2C3&sortBy=DD
```

**⚠️ 7-day window:** Results span the last 7 days. Sort is by date descending (DD), so most-recent are first. Jobs posted within 24h score +1 bonus; jobs posted within 48h score +0.5. This ensures Mondays and slow days still yield 10+ results per role.

Keywords exactly "Staff Software Engineer" / "Senior Software Engineer" — no extra words.

Extract all visible job cards: title, company, location, posted time, job URL. Do NOT visit individual pages yet.

**Company filter:** Keep only jobs from:
OpenAI, Anthropic, Google, Google DeepMind, Meta, Apple, Amazon, Microsoft, Stripe, Databricks, NVIDIA, Groq, Mistral, Cohere, Scale AI, Harvey AI, Fireworks AI, Together AI, Perplexity, Waymo, xAI, Figma, Notion, Vercel, Cloudflare, Snowflake, Palantir, Anduril, Cerebras, Linear, Character.AI, Baseten, Deepgram, LiveKit, Airbnb, Coinbase, Dropbox, Lyft, Uber, Pinterest, Salesforce, Adobe, Intuit, ServiceNow, Workday, MongoDB, Elastic, HashiCorp, Twilio, Okta, CrowdStrike, Palo Alto Networks, SandboxAQ, Turing, Fastly, Atlassian, Zoom, Robinhood

Split into:
- **STEP 1 main list** — top 10 Staff + top 10 Senior (pick most recently posted if more than 10 qualify)
- **STEP 1 remainder list** — every job that passed the company filter but did NOT make the top 10 cap. Store: title, company, location, posted time, URL. **No detail page visits for these.**

Tag source: `🔍 LinkedIn Search`

---

## STEP 1b — Gmail Inbox: LinkedIn Job Alert Emails (last 3 days) — ALWAYS RUN

**Run this step unconditionally** regardless of how many jobs STEP 1 found. Gmail alerts often surface jobs not visible in LinkedIn search (different ranking algorithm). Cap additions at what's needed to reach 10 Staff + 10 Senior total.

Call `gmail_search_messages` with `q: "from:jobs-noreply@linkedin.com newer_than:3d"`, `maxResults: 15`.

For each message, call `gmail_read_message`. Extract job entries **from email body text only** — no page visits. Get: title, company, location, URL (reconstruct from 10-digit ID if needed).

Filter: target company list + Staff/Senior title keywords only. Dedup against STEP 1 URLs.

Split extracted jobs:
- **STEP 1b main additions** — fill up to 10 Staff + 10 Senior cap (combined with STEP 1). Tag `📧 Email Alert`.
- **STEP 1b remainder** — jobs that passed filter but didn't fit the cap. Store title, company, URL only.

Log: `"Gmail: [N] emails scanned, [N] added to main, [N] to remainder"`

---

## STEP 1c — Greenhouse Job Board (Personal Browser, Last 5 Days)

Independent source — no dedup against LinkedIn. No cap sharing with LinkedIn.

### Switch to personal browser
Call `switch_browser` — verify "Personal" in name. If not, call again.
Call `tabs_context_mcp` with `createIfEmpty: true`.

### Navigate and login check
Navigate to `https://my.greenhouse.io/jobs?default=true`. Sleep 4s. Screenshot.
- ✅ Job board visible → proceed
- ❌ Login screen → set `GREENHOUSE_STATUS = "not_logged_in"`. Skip to STEP 2.

### Search (2 queries, 2 pages max each)
Filters: Full Time · last 5 days (or 7) · San Francisco / Remote.
Query 1: `"Staff Software Engineer"` OR `"Staff Engineer"`
Query 2: `"Senior Software Engineer"` OR `"Senior Engineer"`

Extract listings from page text — do NOT visit detail pages yet. Cap raw candidates at **15 total**.

### Pre-filter shortlist (no page visits)
From raw candidates, select top **8** by title relevance (prefer Staff/Principal, infra/privacy/AI keywords in title). These become the **Greenhouse main list**.
Any others (passed filter but not in top 8) → **Greenhouse remainder list** (title, company, URL only).

### Detail pages — score shortlist (max 8 page visits)
For each of the 8, navigate and `get_page_text` once. Score (1–10):
- +3 privacy, GDPR, CCPA, PII, compliance, data governance
- +2 distributed systems, Kubernetes, gateway, service mesh, platform engineering
- +2 AI/ML infra, LLM, MLOps, model serving, inference
- +1 Staff / Principal / L6+
- +1 posted within 3 days

Jobs scoring **< 4** → move to **Greenhouse remainder list** (already have URL, just note score).
Jobs scoring **≥ 4** → stay in Greenhouse main list (max 8).

Extract: first 200 chars description, recruiter/HM if listed, salary if shown.
Tag all: `🏢 Greenhouse`.

Log: `"Greenhouse: [N] raw, [N] shortlisted, [N] scored ≥4 (main), [N] moved to remainder"`

---

## STEP 2 — LinkedIn Detail Page Extraction (final main candidates only)

Take STEP 1 + STEP 1b combined main list. For each job (max 20 total, 10 Staff + 10 Senior), navigate to LinkedIn URL, `get_page_text` once. Extract:

1. **Description snippet** — first **200 chars**
2. **Recruiter / HM** — run this JS via `javascript_tool`:

```javascript
let rN='—',rU='—';
const secs=document.querySelectorAll('section');
for(const s of secs){if(s.innerText&&s.innerText.includes('Meet the hiring team')){const a=s.querySelector('a[href*="/in/"]');if(a){rU=a.href.split('?')[0];const e=a.querySelector('strong')||a.querySelector('span:not(.visually-hidden):not(.sr-only)');rN=e?e.innerText.trim():a.innerText.trim().split('\n')[0].trim();}break;}}
if(rN==='—'){const p=document.querySelector('[data-tracking-control-name*="job_details_people"]');if(p){const a=p.querySelector('a[href*="/in/"]');if(a){rU=a.href.split('?')[0];rN=a.innerText.trim().split('\n')[0].trim();}}}
if(rN==='—'){const t=document.body.innerText;const i=t.indexOf('Meet the hiring team');if(i>-1){const lines=t.substring(i,i+200).split('\n').map(l=>l.trim()).filter(l=>l.length>2&&l.length<60);if(lines.length>1)rN=lines[1];}}
JSON.stringify({name:rN,url:rU});
```

3. **Salary** — if visible
4. **Posted time**
5. **Final relevance score** (1–10):
   - +3 privacy, GDPR, CCPA, PII, data governance, compliance
   - +2 distributed systems, Kubernetes, gateway, service mesh, platform engineering
   - +2 AI/ML infra, LLM, MLOps, model serving, inference
   - +1 Staff / Principal / L6+
   - +1 posted within 24h

Jobs that score **< 3** after reading the description → move to remainder list (already have URL).

---

## STEP 3 — Compose and Send Email

Get today's date: `date "+%B %d, %Y"` via Bash.

### 3a. Create Draft
Call `gmail_create_draft` with **`contentType: "text/html"`** — this is REQUIRED. Without it, Gmail displays raw HTML source code instead of a styled email. Note both **draftId** and **messageId** from the response.
**To:** darshilbhayani92@gmail.com
**Subject:** `🧠 Daily AI & FAANG Job Digest – Bay Area/Remote-US – [TODAY'S DATE]`
**contentType:** `text/html` ← MUST be set explicitly every time; the default is `text/plain` which breaks rendering

### 3b. Personal Browser Validation
1. `switch_browser` → verify "Personal" in name; if not, call again.
2. `tabs_context_mcp` with `createIfEmpty: true`. Get tabId.
3. Navigate to `https://mail.google.com/mail/u/0/`. Sleep 3s.
4. Tab title must contain `darshilbhayani92@gmail.com`:
   - ✅ → proceed to 3c
   - ❌ → log `"Personal Gmail not accessible — draft (ID: [draftId]) saved. Open Gmail → Drafts to send manually."` and stop.

### 3c. Send via Chrome
1. Navigate to `https://mail.google.com/mail/u/0/#drafts/[messageId]` using the **messageId** from step 3a — this opens the exact draft directly and avoids any stale compose windows that may be open from previous runs. Wait 2s. Screenshot.
2. `find` the Send button in the compose window. Click it.
3. Screenshot to confirm "Message sent" toast and that draft count decreased by 1.

---

## EMAIL HTML TEMPLATE

**MANDATORY: All 7 sections must appear in every email. Do not skip any section.**

Sections:
1. 🟣 Header with stats badges
2. 🟢 Section 1 — Top Staff SWE Roles (LinkedIn + Email Alerts) — fully analysed, scored
3. 🔵 Section 2 — Top Senior SWE Roles (LinkedIn + Email Alerts) — fully analysed, scored
4. 🏢 Section 3 — Greenhouse Picks (or login-request box if not logged in)
5. 🔍 Section 4 — Also Found: Quick Links (remainder list — no scoring, just links)
6. 🔥 High-Signal Companies — dynamic list of companies actively posting today with 1-sentence signal
7. 🌟 Emerging AI Startups to Watch — always include all 3 static entries
8. 🏢 LinkedIn Target Company Universe — full table, always shown
9. Footer

```html
<!DOCTYPE html><html><head><meta charset="UTF-8"></head>
<body style="font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f5f5f5;margin:0;padding:20px;">
<div style="max-width:950px;margin:0 auto;background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 2px 12px rgba(0,0,0,0.1);">

  <!-- HEADER -->
  <div style="background:linear-gradient(135deg,#1a1a2e 0%,#16213e 50%,#0f3460 100%);padding:36px 40px;color:#fff;">
    <div style="font-size:26px;font-weight:700;margin-bottom:8px;">🤖 Daily AI Job Digest — [DATE]</div>
    <div style="color:#a0c4ff;font-size:14px;margin-bottom:18px;">Hi Darshil — roles from <strong style="color:#fff;">LinkedIn + email alerts + Greenhouse</strong>, ranked by fit with your <strong style="color:#fff;">privacy infra + backend + AI</strong> background.</div>
    <div style="display:flex;flex-wrap:wrap;gap:8px;">
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🟣 Staff SWE: [N] fully analysed</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🔵 Senior SWE: [N] fully analysed</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🏢 Greenhouse: [N] picks</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🔍 Also found: [N] quick links</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">📧 Email alerts: [N] roles</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🔥 Top Staff: [COMPANY — ROLE]</span>
      <span style="background:rgba(255,255,255,0.12);border-radius:20px;padding:5px 13px;font-size:12px;">🔥 Top Senior: [COMPANY — ROLE]</span>
    </div>
  </div>

  <!-- SECTION 1: STAFF SWE -->
  <div style="padding:32px 40px;">
    <div style="background:#f0ebff;border-radius:8px;padding:14px 20px;margin-bottom:20px;">
      <span style="font-size:17px;font-weight:700;">🟣 Section 1 — Top Staff Software Engineer Roles</span>
    </div>
    <table style="width:100%;border-collapse:collapse;font-size:13px;">
      <thead><tr style="background:#f8f9fa;text-align:left;">
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:28px;">#</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:150px;">Company</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Role / Title</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:100px;">Location</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:90px;">Est. TC</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:55px;">Score</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Why Relevant</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:115px;">Recruiter/HM</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;width:60px;">Apply</th>
      </tr></thead>
      <tbody>
        <tr><td colspan="9" style="background:#fff8e6;padding:7px 12px;font-weight:700;font-size:11px;border:1px solid #e0e0e0;text-transform:uppercase;letter-spacing:.5px;">🔒 Privacy &amp; Security Infrastructure</td></tr>
        <tr>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-weight:600;">[#]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><strong>[Company]</strong><br>
            <span style="background:[BADGE_COLOR];color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;font-weight:700;">[FAANG|AI|UNICORN|STARTUP]</span>
            <span style="background:#e8e8e8;color:#555;border-radius:3px;padding:2px 5px;font-size:10px;margin-left:3px;">[🔍|📧]</span></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><strong>[Role]</strong></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;">[Location]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;">[TC or —]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;text-align:center;"><strong>[N]/10</strong></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-size:12px;">[Why relevant]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-size:12px;"><a href="[RECRUITER_URL]">[Name]</a> or —</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><a href="[JOB_URL]" style="color:#0a66c2;font-weight:600;">Apply →</a></td>
        </tr>
        <!-- Group headers: 🤖 AI & ML Platform · ⚙️ Distributed Systems & Backend · 🔧 Other Engineering -->
      </tbody>
    </table>
  </div>

  <!-- SECTION 2: SENIOR SWE -->
  <div style="padding:0 40px 32px;">
    <div style="background:#e8f4fd;border-radius:8px;padding:14px 20px;margin-bottom:20px;">
      <span style="font-size:17px;font-weight:700;">🔵 Section 2 — Top Senior Software Engineer Roles</span>
    </div>
    <table style="width:100%;border-collapse:collapse;font-size:13px;">
      <thead><tr style="background:#f8f9fa;text-align:left;">
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">#</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Company</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Role / Title</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Location</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Est. TC</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Score</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Why Relevant</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Recruiter/HM</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Apply</th>
      </tr></thead>
      <tbody><!-- Same row structure as Section 1 --></tbody>
    </table>
  </div>

  <!-- SECTION 3: GREENHOUSE PICKS -->
  <!-- Version A (logged in): -->
  <div style="padding:0 40px 32px;">
    <div style="background:#e6f4ea;border-radius:8px;padding:14px 20px;margin-bottom:20px;">
      <span style="font-size:17px;font-weight:700;">🏢 Section 3 — Greenhouse Picks (Last 5 Days · SF · Full Time)</span>
    </div>
    <table style="width:100%;border-collapse:collapse;font-size:13px;">
      <thead><tr style="background:#f8f9fa;text-align:left;">
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">#</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Company</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Role / Title</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Location</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Salary</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Score</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Why Relevant</th><th style="padding:9px 8px;border:1px solid #e0e0e0;">Recruiter/HM</th>
        <th style="padding:9px 8px;border:1px solid #e0e0e0;">Apply</th>
      </tr></thead>
      <tbody>
        <tr>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-weight:600;">[#]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><strong>[Company]</strong><br><span style="background:#16a34a;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;font-weight:700;">🏢 Greenhouse</span></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><strong>[Role]</strong></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;">[Location]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;">[Salary or —]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;text-align:center;"><strong>[N]/10</strong></td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-size:12px;">[Why relevant]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;font-size:12px;">[Name or —]</td>
          <td style="padding:10px 8px;border:1px solid #e0e0e0;"><a href="[URL]" style="color:#16a34a;font-weight:600;">Apply →</a></td>
        </tr>
      </tbody>
    </table>
  </div>
  <!-- Version B (not logged in) — replace Version A with this: -->
  <div style="padding:0 40px 32px;">
    <div style="background:#e6f4ea;border-radius:8px;padding:14px 20px;margin-bottom:16px;"><span style="font-size:17px;font-weight:700;">🏢 Section 3 — Greenhouse Picks</span></div>
    <div style="border:2px dashed #f59e0b;border-radius:8px;padding:24px 28px;background:#fffbeb;">
      <div style="font-size:15px;font-weight:700;color:#b45309;margin-bottom:10px;">⚠️ Action Required: Greenhouse Login Needed</div>
      <p style="margin:0 0 10px;font-size:13px;color:#444;">Greenhouse (<strong>my.greenhouse.io</strong>) was not accessible — no active login session found.</p>
      <ol style="margin:0 0 14px;padding-left:20px;font-size:13px;color:#444;line-height:1.8;">
        <li>Open your <strong>personal Chrome browser</strong></li>
        <li>Go to <a href="https://my.greenhouse.io" style="color:#0a66c2;">https://my.greenhouse.io</a> and log in</li>
        <li>Stay logged in — the agent auto-detects the session on the next run</li>
      </ol>
    </div>
  </div>

  <!-- SECTION 4: ALSO FOUND — QUICK LINKS (remainder list) -->
  <div style="padding:0 40px 32px;">
    <div style="background:#f3f4f6;border-radius:8px;padding:14px 20px;margin-bottom:16px;">
      <span style="font-size:17px;font-weight:700;">🔍 Section 4 — Also Found: Quick Links</span>
      <span style="font-size:12px;color:#666;margin-left:10px;">Matched your company list but not fully searched — check manually if interested</span>
    </div>
    <table style="width:100%;border-collapse:collapse;font-size:13px;">
      <thead><tr style="background:#f8f9fa;text-align:left;">
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:28px;">#</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:160px;">Company</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;">Role / Title</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:110px;">Location</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:90px;">Posted</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:80px;">Source</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;width:65px;">Link</th>
      </tr></thead>
      <tbody>
        <tr style="border-bottom:1px solid #f0f0f0;">
          <td style="padding:8px 10px;border:1px solid #e0e0e0;color:#888;">[#]</td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;"><strong>[Company]</strong></td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;">[Role Title]</td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;color:#666;">[Location]</td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;color:#888;font-size:12px;">[Posted time]</td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;">
            <span style="background:#e8e8e8;color:#555;border-radius:3px;padding:2px 6px;font-size:10px;">🔍 LinkedIn</span>
            <span style="background:#dbeafe;color:#1d4ed8;border-radius:3px;padding:2px 6px;font-size:10px;">📧 Alert</span>
            <span style="background:#dcfce7;color:#166534;border-radius:3px;padding:2px 6px;font-size:10px;">🏢 GH</span>
          </td>
          <td style="padding:8px 10px;border:1px solid #e0e0e0;"><a href="[JOB_URL]" style="color:#6b7280;font-size:12px;">View →</a></td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- ████ HIGH-SIGNAL COMPANIES — ALWAYS INCLUDE ████ -->
  <div style="padding:0 40px 28px;">
    <div style="background:#fff3e0;border-radius:8px;padding:20px 24px;">
      <div style="font-size:16px;font-weight:700;margin-bottom:12px;">🔥 High-Signal Companies — Active Hiring Today</div>
      <p style="margin:6px 0;font-size:13px;color:#444;">List every company from today's results that is actively posting — 1 concise hiring signal sentence per company. Example format:</p>
      <p style="margin:6px 0;font-size:13px;">• <strong>[Company]</strong> — [What they're hiring for and why it's relevant to Darshil's background]. [Post recency if notable, e.g. "Posted 2h ago."]</p>
      <p style="margin:6px 0;font-size:13px;">• <strong>[Company]</strong> — [Signal sentence].</p>
    </div>
  </div>

  <!-- ████ EMERGING AI STARTUPS — ALWAYS INCLUDE ████ -->
  <div style="padding:0 40px 28px;">
    <div style="background:#e8f5e9;border-radius:8px;padding:20px 24px;">
      <div style="font-size:16px;font-weight:700;margin-bottom:12px;">🌟 Emerging AI Startups to Watch</div>
      <p style="margin:6px 0;font-size:13px;">• <strong>Baseten</strong> — ML model inference &amp; deployment platform ($5B valuation, NVIDIA-backed); strong fit for distributed systems + model-serving infra engineers.</p>
      <p style="margin:6px 0;font-size:13px;">• <strong>LiveKit</strong> — Real-time voice/video AI infrastructure (Series C, $1B+); hiring backend engineers with Kubernetes + WebRTC + low-latency systems expertise.</p>
      <p style="margin:6px 0;font-size:13px;">• <strong>Fireworks AI</strong> — Fast open-source LLM serving API; explicitly values traffic gateway, load balancing, and low-latency infra background.</p>
    </div>
  </div>

  <!-- COMPANY UNIVERSE TABLE — ALWAYS INCLUDE -->
  <div style="padding:0 40px 32px;">
    <div style="font-size:16px;font-weight:700;margin-bottom:14px;">🏢 LinkedIn Target Company Universe Scanned Today</div>
    <table style="width:100%;border-collapse:collapse;font-size:12px;">
      <thead><tr style="background:#f8f9fa;">
        <th style="padding:8px 10px;border:1px solid #e0e0e0;text-align:left;">Company</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;text-align:left;width:100px;">Category</th>
        <th style="padding:8px 10px;border:1px solid #e0e0e0;text-align:left;">Description</th>
      </tr></thead>
      <tbody>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">OpenAI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Creator of GPT-4 and ChatGPT; leading AGI research lab</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Anthropic</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Safety-focused AI lab; maker of Claude</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Google DeepMind</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Google's consolidated AI research arm; Gemini models</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Meta AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Open-source AI (Llama); social + VR platform AI</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Apple</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">On-device AI, privacy-first ML, Apple Intelligence</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Amazon</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">AWS cloud, Alexa AI, Bedrock foundation models</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Microsoft</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Azure cloud, OpenAI partnership, Copilot ecosystem</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">NVIDIA</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">GPU leader; CUDA, DGX, AI infrastructure hardware</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Stripe</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Global payments infra with AI fraud detection</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Databricks</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Unified data + AI platform; Delta Lake, MLflow</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Cloudflare</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Global network security, CDN, edge AI platform</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Snowflake</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Cloud data warehouse; Snowpark ML and AI</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Waymo</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Alphabet's fully autonomous driving company</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Palantir</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#ff6b35;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">FAANG</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Data analytics and AI for enterprise + government</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Anduril</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Defense AI; autonomous weapons and sensor systems</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Groq</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Ultra-fast LLM inference chips (LPU architecture)</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Scale AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">AI data labeling and RLHF infrastructure at scale</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Cohere</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Enterprise NLP and LLM APIs for business</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Harvey AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">AI legal research and drafting for law firms</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Perplexity</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">AI-powered answer engine and search</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">xAI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Elon Musk's AI lab; Grok LLM</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Cerebras</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Wafer-scale AI chips; ultra-fast LLM training</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Fireworks AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Fast open-source LLM serving API platform</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Together AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Collaborative AI research and model training cloud</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Character.AI</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Personalized AI companions and chatbots</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Baseten</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">ML model deployment and inference infra ($5B)</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Deepgram</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Voice AI and speech recognition infrastructure</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">LiveKit</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#3b82f6;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">STARTUP</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Real-time voice/video AI developer infrastructure</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Figma</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Collaborative design tool with AI features</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Notion</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#f59e0b;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">UNICORN</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">All-in-one workspace with AI writing assistant</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Vercel</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#3b82f6;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">STARTUP</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Frontend cloud platform for Next.js</td></tr>
        <tr style="background:#f9f9f9;"><td style="padding:7px 10px;border:1px solid #e0e0e0;">Linear</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#3b82f6;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">STARTUP</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">Modern project management for software teams</td></tr>
        <tr><td style="padding:7px 10px;border:1px solid #e0e0e0;">Mistral</td><td style="padding:7px 10px;border:1px solid #e0e0e0;"><span style="background:#7c3aed;color:#fff;border-radius:3px;padding:2px 6px;font-size:10px;">AI</span></td><td style="padding:7px 10px;border:1px solid #e0e0e0;">European open-weight LLM research lab</td></tr>
      </tbody>
    </table>
  </div>

  <!-- FOOTER -->
  <div style="background:#f8f9fa;padding:20px 40px;text-align:center;color:#888;font-size:12px;border-top:1px solid #e0e0e0;">
    Sent automatically · LinkedIn Search (7-day) + Email Alerts + Greenhouse · darshilbhayani92@gmail.com
  </div>
</div>
</body></html>
```

**Badge colors:** FAANG `#ff6b35` · AI `#7c3aed` · UNICORN `#f59e0b` · STARTUP `#3b82f6` · Greenhouse `#16a34a`

---

## SUCCESS CRITERIA
1. Global budget respected: ≤10 Staff + ≤10 Senior LinkedIn jobs, ≤8 Greenhouse, ≤25 detail page visits, ~20k tokens
2. **Target: 10 jobs per role.** Use 7-day LinkedIn window + 2 scroll passes + unconditional Gmail scraping to reach this. If still under 10 after all sources, include all found and note the gap.
3. **Remainder list populated:** ALL jobs that passed company/title filter but didn't make top-10 cut appear in Section 4 as quick links — no waste
4. Section 4 uses only data already extracted from search pages — zero extra page visits
5. Source badges on all rows: 🔍 LinkedIn · 📧 Alert · 🏢 GH
6. Every main-list job has real apply URL; every Section 4 job has real job URL
7. Recruiter JS run on every LinkedIn detail page in the main list
8. Greenhouse login check via screenshot; Version B fallback if not logged in
9. Personal browser validated as `darshilbhayani92@gmail.com` before sending
10. **All 7 email sections are present:** Header · Section 1 (Staff) · Section 2 (Senior) · Section 3 (Greenhouse) · Section 4 (Also Found) · High-Signal Companies · Emerging Startups · Company Universe · Footer — **never skip any section**
11. Email sent with `contentType: "text/html"` (REQUIRED); draft saved with manual note if browser validation fails