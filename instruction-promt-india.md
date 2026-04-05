## Daily Job Digest — Staff / Senior SWE — Bangalore, India (LinkedIn Only)



**Objective:** Find and email the top 15 currently open Staff Engineer and Senior Software Engineer jobs in Bangalore and India, sourced exclusively from LinkedIn. Include referral-finding tips and cold outreach options. Send via Chrome (not just draft).



**User:** Darshil Bhayani — darshilbhayani92@gmail.com



---



### Step 1: Run 2 targeted LinkedIn job searches (no page fetches)



**Search 1 — LinkedIn Staff Engineer roles in India:**

```

site:linkedin.com/jobs "staff software engineer" OR "staff engineer" Bangalore OR Bengaluru OR India 2026

```



**Search 2 — LinkedIn Senior SWE roles at product companies in Bangalore:**

```

site:linkedin.com/jobs "senior software engineer" OR "senior engineer" Bangalore OR Bengaluru India product company 2026

```



**Rules:**

- ✅ Extract company name, job title, and direct LinkedIn job URL from search snippets only

- ✅ Prioritize product companies, AI/ML startups, unicorns, and global tech offices

- ✅ Prefer recently posted roles (look for "new" signals in snippets)

- ❌ Do NOT fetch any individual job posting page

- ❌ Do NOT run more than 2 searches

- ❌ Do NOT include IT staffing/outsourcing companies (TCS, Wipro, Infosys, HCL unless product role)



---



### Step 2: Compile top 15 results



Rank by priority:

1. AI-first / ML companies (Sarvam AI, Krutrim, Yellow.ai, etc.)

2. Indian unicorns / product companies (PhonePe, Razorpay, CRED, Groww, Meesho, Zepto, Swiggy, BrowserStack, Juspay, Postman, etc.)

3. Global tech with strong India offices (Google, Microsoft, Coinbase, Databricks, Uber, Atlassian, Stripe, NVIDIA, etc.)



---



### Step 3: Build referral + cold outreach section



For each company in the top 15, include:

- **Referral search query** (LinkedIn search Darshil can run to find a 1st/2nd degree connection)

- **Who to cold outreach** (Engineering Manager / VP Eng / Director Eng title)

- **LinkedIn search to find that person**



**Referral ask template (include in email):**

```

Hi [Name],

Hope you're doing well! I came across the [Role] opening at [Company] and I'm genuinely excited about it — [one specific thing about the company].

I have [X] years of experience in [relevant skills], most recently at [Company] where I [one-line impact].

Would you be open to referring me or pointing me to the right person? Happy to share my resume. Thanks so much!

— Darshil

```



**Cold outreach template (include in email):**

```

Hi [Name],

I'm a Staff / Senior Software Engineer with [X] years in [distributed systems / backend / platform]. I've been following [Company]'s work on [specific thing] and it really impressed me.

At [Last Company], I [specific achievement with numbers]. I'd love to explore if there's a fit — even a 15-minute call would mean a lot.

— Darshil Bhayani | linkedin.com/in/darshilbhayani

```



---



### Step 4: Create rich HTML email draft via Gmail MCP



Use tool: `mcp__1f8d0890-717b-4d71-bd50-8024bcb9489c__gmail_create_draft`



- **To:** darshilbhayani92@gmail.com

- **Subject:** `🇮🇳 Top 15 Staff/Senior SWE Jobs — Bangalore & India | {Today's Date}`

- **contentType:** text/html

- **Body:** Full rich HTML email with:

  - Blue header table (#1a56db) for jobs: #, Company, Role, Level badge (Staff=amber, Senior=green), Location, LinkedIn Apply Link

  - Orange tip box: "Apply within 24–48 hours — applicant pools are smallest in the first 2 days"

  - Referral strategy table: Company | How to find a referrer | LinkedIn search query

  - Copy-paste referral ask message (green left-border box)

  - Cold outreach table: Company | Who to target | LinkedIn search to find them

  - Copy-paste cold outreach message (green left-border box)

  - Footer: "This digest runs automatically every weekday at 8:05 AM — Daily Job Digest Agent, Bangalore India"



Save the returned `draftId` and `threadId` for the next step.



---



### Step 5: Send the email via Chrome (REQUIRED — do not skip)



After creating the draft, use Chrome to actually send it:



1. Use `mcp__Claude_in_Chrome__tabs_context_mcp` with `createIfEmpty: true` to get a tab ID

2. Navigate to `https://mail.google.com/mail/u/0/#drafts` using `mcp__Claude_in_Chrome__navigate`

3. Wait 2 seconds, then use `mcp__Claude_in_Chrome__javascript_tool` to find and click the draft:

```javascript

const rows = document.querySelectorAll('tr.zA');

// Find the row matching our subject

let targetRow = null;

for (const row of rows) {

  if (row.innerText?.includes('Bangalore & India')) {

    targetRow = row;

    break;

  }

}

if (targetRow) { targetRow.click(); 'Clicked draft row'; }

else { rows[0]?.click(); 'Clicked first row as fallback'; }

```

4. Wait 2 seconds for compose window to open, then use `mcp__Claude_in_Chrome__javascript_tool` to send:

```javascript

const allButtons = document.querySelectorAll('[role="button"], button');

let sendBtn = null;

for (const btn of allButtons) {

  const tooltip = btn.getAttribute('data-tooltip') || '';

  const label = btn.getAttribute('aria-label') || '';

  if (tooltip.includes('Send') || label.includes('Send')) {

    sendBtn = btn;

    break;

  }

}

if (sendBtn) {

  const evt = new MouseEvent('click', { bubbles: true, cancelable: true, view: window });

  sendBtn.dispatchEvent(evt);

  'Clicked send';

} else { 'Send button not found'; }

```

5. Verify success: use `mcp__Claude_in_Chrome__javascript_tool` to check for "Message sent" toast:

```javascript

const toasts = Array.from(document.querySelectorAll('span, div')).filter(el => el.innerText?.trim() === 'Message sent');

toasts.length > 0 ? 'Email sent successfully!' : 'Toast not found — may still have sent';

```



---



### Step 6: Confirm back to user



After sending, report:

- ✅ Email sent to darshilbhayani92@gmail.com

- Number of direct LinkedIn job links vs career page links

- How many Staff vs Senior roles found

- Any standout newly posted roles worth acting on fast
