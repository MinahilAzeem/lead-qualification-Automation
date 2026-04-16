# LeadIQ — AI Lead Qualification System

> Automatically qualifies every inbound lead in under 2 minutes using AI — scores them Hot, Warm, or Cold, sends a personalized follow-up email, and logs everything to a dashboard. Built as a portfolio project for my AI automation agency.

---

## Live Demo

- **Web App:** [LeadIQ on Lovable](https://5431e449-2c16-40a3-ae77-2fda2f879c62.lovableproject.com)
- **Demo Video:** [Watch 3-min walkthrough on Loom] (https://www.loom.com/share/3cf67fed7a1147a8b9acbd3ce70f87ca)

---

## What It Does

Most businesses lose 40–60% of leads simply because they respond too slowly. LeadIQ solves this by:

- Receiving a lead the moment they submit a contact form
- Sending it to an AI (Google Gemini) that scores the lead as **HOT**, **WARM**, or **COLD**
- Automatically sending a personalized follow-up email within 2 minutes
- Logging the lead with score, reason, and timestamp to Google Sheets
- Optionally notifying the business owner via Slack for hot leads

The business owner wakes up in the morning and already knows which leads to call first.

---

## System Architecture

```
Contact Form (Lovable)
        ↓
  n8n Webhook (trigger)
        ↓
  Google Gemini AI (score lead)
        ↓
  ┌─────────────────────┐
  │  HOT / WARM / COLD  │
  └─────────────────────┘
        ↓            ↓
  Gmail (auto      Google Sheets
  send email)      (log lead)
        ↓
  Slack alert (hot leads only, optional)
```

---

## Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Frontend | Lovable (React + Tailwind) | Contact form + dashboard UI |
| Automation | n8n | Workflow orchestration |
| AI | Google Gemini 2.5 Flash | Lead scoring + email personalization |
| Email | Gmail via n8n | Automated follow-up |
| Database | Google Sheets | Lead logging and dashboard data |
| Tunnel (dev) | ngrok | Local n8n exposure for testing |
| Hosting (prod) | Railway | n8n cloud hosting |

---

## How the AI Scores Leads

The Gemini prompt evaluates each lead on three signals:

- **Budget** — $2,000+ scores higher
- **Intent** — specific problem description scores higher than vague inquiries  
- **Urgency** — "ASAP" or "within a month" scores higher than "just exploring"

It returns a structured JSON response:

```json
{
  "score": "HOT",
  "reason": "Budget aligns with project scope and urgency is clear",
  "email_opener": "Hi Sarah, we received your urgent request and are ready to help"
}
```

---

## Project Structure

```
leadiq/
├── README.md
├── workflow/
│   └── leadiq-n8n-workflow.json     ← import this into your n8n
├── docs/
│   ├── architecture-diagram.png
│   └── setup-guide.md
└── screenshots/
    ├── contact-form.png
    ├── dashboard.png
    └── google-sheets.png
```

---

## Setup Guide

### Prerequisites
- n8n account (self-hosted or n8n.cloud)
- Google account (Gmail + Sheets)
- Google Gemini API key (free at aistudio.google.com)
- ngrok (for local development)

### Step 1 — Import the workflow
1. Open your n8n instance
2. Click **Workflows → Import**
3. Upload `workflow/leadQualifier.json`

### Step 2 — Add credentials
In n8n, add these credentials:
- Google Gemini API key
- Gmail OAuth2
- Google Sheets OAuth2

### Step 3 — Update the webhook URL
In `ContactForm.tsx`, replace the webhook URL with your own n8n webhook URL.

### Step 4 — Create your Google Sheet
Create a sheet with these headers in row 1:
```
Name | Email | Message | Budget | Timeline | Score | Reason | Date
```

### Step 5 — Test end to end
1. Click **Listen for test event** on the webhook node
2. Submit the contact form
3. Watch the lead appear in your sheet within 2 minutes


---

## Results

| Metric | Before | After |
|--------|--------|-------|
| Lead response time | 4–6 hours | Under 2 minutes |
| Manual follow-up effort | 30 min/day | 0 min/day |
| Lead scoring accuracy | Manual / guesswork | AI-powered, consistent |

---

## About

Built by **Minahil** 
```
I build AI-powered automation systems for small businesses that save time and increase revenue.
```

**Contact:** minahilazeembscs@gmail.com                                       
**LinkedIn:** [your LinkedIn URL here]


---

## License

MIT — free to use and adapt for your own projects.
