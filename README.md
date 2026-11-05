# AuditReach AI

**Project:** Lead Generation and Outreach Agent (n8n workflow)

Enter a business type and location. The AI finds leads, audits each website and sends a personalized cold email that highlights the site's weak points.

## What it does

AuditReach AI is an end-to-end lead generation and outreach agent. It scrapes businesses from Google Maps, extracts an email from each website, then uses Gemini to audit the site (SEO, UX, missing business elements, AI opportunities). The audit findings are turned into a personalized cold email pitching AI automation agency services. The email is sent through Gmail and everything is tracked in a Google Sheet.

## Workflow

```
Form -> Apify Maps Scraper -> Filter (has website) -> Email Extractor (Gemini)
     -> If email valid -> Save lead to Sheet -> Loop over leads
          -> Wait -> Website Weak Points Analyzer (Gemini)
          -> Cold Email Generator (Gemini) -> Gmail Send
          -> Update Sheet (status, weak points, suggestions) -> next lead
```

| Phase | Nodes | Description |
|-------|-------|-------------|
| Input | On form submission | `Business Type`, `Location`, `Lead Number` and `Email Style` (Friendly, Professional or Simple) |
| Scrape | Apify Scraper, Filter | Gets leads from Google Maps and keeps only those with a website |
| Enrich | Information Extractor | Finds the best email on the website and skips invalid ones |
| Store | Append row in sheet | Saves company, category, website, phone, email, address and country |
| Audit | Weak points Analyzer | Audits SEO, UX, business elements, technical issues and AI trends, and suggests improvements |
| Write | Information Extractor & Generator | Writes the subject and body in the selected tone |
| Send | Send a message (Gmail) | Sends the email |
| Track | Append or update row | Updates `Cold Mail Status`, `SEND Time`, `Weak Points` and `Improvments` |

## Setup

### Requirements
- n8n (cloud or self-hosted)
- Apify API token
- Google Gemini API key
- Gmail (OAuth2) and Google Sheets (OAuth2)

### Steps
1. In n8n go to **Workflows -> Import from File** and select `workflow.json`.
2. In the **Apify Scraper** node, replace `YOUR_APIFY_TOKEN` with your own token. Better still, use an n8n credential or environment variable instead of hardcoding it in the URL.
3. Connect credentials: Google Gemini (PaLM), Google Sheets and Gmail.
4. Create a Google Sheet with these columns: `Company Name`, `Category`, `Website`, `Phone Number`, `Email Address`, `Address`, `Country`, `Cold Mail Status`, `SEND Time`, `Weak Points`, `Improvments`, then select it in both Sheet nodes.
5. Update your name and signature in the **Information Extractor & Generator** prompt.
6. Activate the workflow, open the form and start a lead run.

## Customization

- Edit the services list in the email generator prompt to match your agency.
- Add new tones by adding options to the form dropdown.
- Add or remove audit categories in the **Weak points Analyzer** prompt.

## Notes

- The audit is AI-generated from the site content, not a full technical scan, so spot-check claims before sending emails at scale.
- Follow the email laws of your region (CAN-SPAM, GDPR).

## Author

Muhammad Daoud, AI Automation Engineer
