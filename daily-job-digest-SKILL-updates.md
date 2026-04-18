# Daily Job Digest — SKILL.md Updates

Two changes to apply to your SKILL.md at:
`.claude/skills/daily-job-digest/SKILL.md`

---

## CHANGE 1 — Fix USA Volume (Page 2 loading + better scroll)

**WHY IT WAS ONLY GETTING 5–7 JOBS:**
LinkedIn's virtual scroll requires TWO separate JavaScript calls — the gap between calls is what actually triggers lazy-load to render more cards. When both scroll+extract happen in a single call, only the initially-visible ~7 cards are extracted. The fix: scroll in Call 1, extract in Call 2, then load `&start=25` for page 2.

**FIND this block** (starting around line 110):

```
## CRITICAL: HOW TO EXTRACT JOBS FROM LINKEDIN (READ THIS FIRST)

LinkedIn uses **virtual scrolling** — job cards below the fold are not rendered until you scroll to them. You MUST scroll the job list panel to load all cards before extracting. Use this JavaScript pattern on EVERY search page after navigating:

```javascript
// Step A: Scroll the job list panel 5 times to load all cards
const panel = document.querySelector('.scaffold-layout__list');
if (panel) {
  for (let i = 1; i <= 5; i++) {
    panel.scrollTop = panel.scrollHeight * (i / 5);
  }
}
```

Wait ~2 seconds after scrolling (use a second JavaScript call) then extract jobs:

```javascript
// Step B: Extract all loaded job cards
const cards = document.querySelectorAll('li[data-occludable-job-id]');
...
results.join('\n');
```

If a search URL returns 0 results after scrolling (no matching jobs in the 72h window), move on to the next URL — do NOT skip scrolling.
```

**REPLACE WITH:**

```
## CRITICAL: HOW TO EXTRACT JOBS FROM LINKEDIN (READ THIS FIRST)

LinkedIn uses **virtual scrolling** — job cards below the fold are not rendered until you scroll to them. You MUST scroll in **two separate JavaScript calls** (the gap between calls is what triggers lazy-load to render new cards). Use this 3-step pattern on EVERY search page:

### Step A — Scroll (javascript_tool Call 1 of 2)
Run this as the FIRST javascript_tool call. Make 8 passes to the bottom:
```javascript
// CALL 1 — scroll only, do NOT extract yet
const panel = document.querySelector('.scaffold-layout__list');
if (panel) {
  panel.scrollTop = 0;
  for (let i = 1; i <= 8; i++) {
    panel.scrollTop = panel.scrollHeight * (i / 8);
  }
  panel.scrollTop = panel.scrollHeight;
  'Scrolled — cards visible so far: ' + document.querySelectorAll('li[data-occludable-job-id]').length;
} else { 'No panel'; }
```

### Step B — Extract (javascript_tool Call 2 of 2)
Run this as a **separate, second** javascript_tool call (making a new call gives the DOM time to render lazy-loaded cards):
```javascript
// CALL 2 — extract all cards + capture LinkedIn fit signal
const cards = document.querySelectorAll('li[data-occludable-job-id]');
const seen = new Set();
const results = [];
cards.forEach(card => {
  const jobId = card.getAttribute('data-occludable-job-id');
  if (!jobId || seen.has(jobId)) return;
  seen.add(jobId);
  const titleLink = card.querySelector('a.job-card-list__title--link');
  const title = titleLink?.getAttribute('aria-label')?.replace(/ with verification$/i, '').trim() || 'N/A';
  const company = card.querySelector('.artdeco-entity-lockup__subtitle')?.textContent?.replace(/\s+/g, ' ').trim() || 'N/A';
  const location = card.querySelector('.artdeco-entity-lockup__caption')?.textContent?.replace(/\s+/g, ' ').trim() || 'N/A';
  // Capture LinkedIn applicant fit signal
  const fitRaw = card.querySelector('.job-card-container__job-insight-text, .job-card-list__insight-text, .jobs-unified-top-card__applicant-fit, [class*="job-insight"]')?.textContent?.replace(/\s+/g, ' ').trim() || '';
  let fitScore = 0;
  if (/top applicant/i.test(fitRaw)) fitScore = 3;
  else if (/job match is high/i.test(fitRaw)) fitScore = 2;
  else if (/job match is medium/i.test(fitRaw)) fitScore = 1;
  // "job match is low" → fitScore = 0 (no bonus)
  if (title !== 'N/A') results.push(jobId + '|' + title + '|' + company + '|' + location + '|fit:' + fitScore);
});
'Count: ' + results.length + '\n' + results.join('\n');
```

### Step C — Load Page 2 (MANDATORY for every search URL)
After extracting page 1, **always** navigate to page 2 by appending `&start=25` to the same URL, repeat Steps A+B, and merge results into the same de-duplicated pool. Example:
- Page 1: `https://www.linkedin.com/jobs/search/?keywords=Staff+Software+Engineer&...&sortBy=DD`
- Page 2: `https://www.linkedin.com/jobs/search/?keywords=Staff+Software+Engineer&...&sortBy=DD&start=25`

This doubles the card pool per search and is the primary fix for the "only 5–7 jobs" problem.

If a search URL returns 0 results after both calls, move on to the next URL.
```

---

## CHANGE 2 — LinkedIn Fit Signal Scoring (50% weightage)

**WHY:** LinkedIn's "You'd be a top applicant" / "More likely to hear back" signals are strong quality signals — they tell you not just company tier but also how well YOUR profile matches this specific role. This should heavily influence ranking.

**FIND this block** (Gate 3, around line 248):

```
**Gate 3 — Relevance score (1–10):**
- +3 if FAANG / Top AI Lab (Google, Meta, Apple, Amazon, Microsoft, OpenAI, Anthropic, DeepMind, xAI, Mistral, Cohere, NVIDIA, LinkedIn, etc.)
- +2 if Unicorn / High-growth startup (Stripe, Databricks, Figma, Anduril, Scale AI, Waymo, Rippling, Zscaler, Palo Alto Networks, Databricks, etc.)
- +1 if Enterprise MNC (Infosys, TCS, Wipro, Cognizant, Capgemini, LTIMindtree, etc.)
- +2 if role involves LLM / Generative AI / ML systems / distributed systems / data infra / platform engineering
- +1 if Staff or above
- +1 if salary/TC listed
- +1 if JD mentions Python, Go, Rust, distributed systems, ML infra, data pipelines, platform engineering, Kubernetes, cloud infra

**Keep jobs scoring ≥ 5.** If a section still has fewer than 10 jobs after applying the ≥5 gate, lower the threshold to ≥4 for that section only, and note "(threshold lowered to ≥4)" in the section header.
```

**REPLACE WITH:**

```
**Gate 3 — Relevance score (1–13, 50% weight from LinkedIn fit signal):**

**Group A — Company & Role Quality (0–7 pts):**
- +3 if FAANG / Top AI Lab (Google, Meta, Apple, Amazon, Microsoft, OpenAI, Anthropic, DeepMind, xAI, Mistral, Cohere, NVIDIA, LinkedIn, etc.)
- +2 if Unicorn / High-growth startup (Stripe, Databricks, Figma, Anduril, Scale AI, Waymo, Rippling, Zscaler, Palo Alto Networks, Databricks, etc.)
- +1 if Enterprise MNC (Infosys, TCS, Wipro, Cognizant, Capgemini, LTIMindtree, etc.)
- +2 if role involves LLM / Generative AI / ML systems / distributed systems / data infra / platform engineering
- +1 if Staff or above
- +1 if salary/TC listed
- +1 if JD mentions Python, Go, Rust, distributed systems, ML infra, data pipelines, platform engineering, Kubernetes, cloud infra

**Group B — LinkedIn Profile Fit Signal (0–6 pts, ~50% of total weight):**
- +6 if LinkedIn shows 🟢 "You'd be a top applicant" (extracted as fit:3 → multiply by 2)
- +4 if LinkedIn shows 🟡 "Job match is high" (extracted as fit:2 → multiply by 2)
- +2 if LinkedIn shows "Job match is medium" (extracted as fit:1 → multiply by 2)
- +0 if LinkedIn shows "Job match is low" or no fit signal shown

**Final score = Group A pts + Group B pts** (max 13).

**Sorting:** Always sort by final score descending. When two jobs have the same Group A score, the LinkedIn fit signal is the tiebreaker.

**Keep jobs scoring ≥ 5.** If a section still has fewer than 10 jobs after applying the ≥5 gate, lower the threshold to ≥4 for that section only, and note "(threshold lowered to ≥4)" in the section header.

**Display in email:** Show the LinkedIn fit badge in the Company cell below the company tier badge:
- 🟢 Top Applicant  → `<span style="background:#16a34a;color:#fff;padding:2px 7px;border-radius:4px;font-size:10px;font-weight:600;">🟢 Top Applicant</span>`
- 🟡 High Match     → `<span style="background:#d97706;color:#fff;padding:2px 7px;border-radius:4px;font-size:10px;font-weight:600;">🟡 High Match</span>`
- ⚪ Medium Match   → `<span style="background:#94a3b8;color:#fff;padding:2px 7px;border-radius:4px;font-size:10px;font-weight:600;">⚪ Medium Match</span>`
- (No badge if "Job match is low" or fit:0)
```

---

## QUICK SUMMARY OF WHAT CHANGES

| # | What | Why |
|---|------|-----|
| 1 | Scroll in Call 1, extract in Call 2 (separate JS calls) | DOM lazy-load fires between calls → 25+ cards instead of 7 |
| 2 | `&start=25` page 2 loading for every URL | Doubles pool per search (50 cards per URL instead of 25) |
| 3 | 8 scroll passes instead of 5 | More reliable bottom-of-list reach |
| 4 | `fit:N` appended to extracted row data | Carries LinkedIn fit signal forward for scoring |
| 5 | Group B scoring (+0 to +6 based on fit) | Adds ~50% weight from LinkedIn's own match signal |
| 6 | Fit badge in email Company cell | Visual signal in the digest email |

After applying these changes, the next run should get 20–25 Bay Area jobs per search phase instead of 5–7.
