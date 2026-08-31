Automated Lead Intake, AI Follow-Up & Job Watcher System

Tools used: n8n (self-hosted), Baserow, Google Sheets/Forms, Gmail, Abstract API, Google Gemini, RSS

The Problem

Manually tracking new leads and job opportunities is easy to fall behind on. Entries pile up, get lost, and follow-ups slip through the cracks. There's no automatic way to know when someone new comes in, whether their contact info is valid, what to say to them, or when a relevant job posting appears.

What I Built

Three interlinked automations that turn simple inputs into a self-maintaining system:

1. Intake Pipeline — triggers from either a Google Form or a direct webhook:

Captures new submissions from two sources: a Google Sheets-linked form, or a webhook that any external tool/site can POST to
Normalizes both input shapes into a consistent internal format before processing
Validates and enriches the email address using the Abstract API (deliverability status, MX records, sender/organization data), with request batching to stay within free-tier rate limits
Uses Google Gemini to generate a short summary of the contact and a draft follow-up email
Writes the enriched, AI-summarized record into a Baserow CRM table
Sends a real-time email notification with the new contact's details and AI-generated draft

![Intake pipeline](Lead-Tracker-Flow)
[Download intake-workflow.json](Lead-Tracker-via-Google-Form-and-Webhooks.json)

2. Follow-Up Scheduler — runs independently on a daily timer:

Queries the CRM for contacts still marked "New"
Filters for entries older than 3 days with no status update
Sends a reminder email listing exactly who still needs follow-up

3. Job-Board Watcher — a separate scheduled workflow:

Polls a We Work Remotely RSS feed on a timer
Filters postings by keyword relevance (automation, no-code, RevOps, etc.)
Deduplicates against previously-seen postings using n8n's built-in duplicate tracking, so re-runs never create repeat entries
Writes new matches to a dedicated Jobs table in Baserow
Sends a notification for each genuinely new match
Skills Demonstrated
API integration — calling third-party REST APIs, handling authentication, parsing nested JSON, and respecting rate limits with batching
Multi-source data normalization — unifying two differently-shaped trigger inputs (form data vs. webhook payload) into one consistent format
AI integration — using an LLM (Google Gemini) within an automation pipeline to generate contextual summaries and draft content
Data enrichment — combining data from multiple upstream sources into a single unified record
Conditional logic & deduplication — filtering records based on string/date comparisons, and preventing duplicate processing across scheduled runs
Scheduled automation — independent, timer-driven workflows separate from event-driven pipelines
Self-hosting & deployment — running n8n via Docker rather than relying solely on a hosted cloud service
Debugging real-world data shapes — nested API responses, Baserow's single-select field objects, date formatting requirements, cross-node data referencing, and n8n's item-based (not batch-based) execution model
Outcome

New contacts are automatically validated, enriched, summarized by AI, and logged with zero manual data entry. Stale leads surface automatically instead of relying on memory. Relevant job postings are collected and deduplicated without any manual checking. The system runs continuously in the background across three independent workflows, and building it required debugging real API errors, nested data structures, field-type mismatches, and architectural rework (moving from a per-item lookup to a purpose-built deduplication approach) — the same kinds of problems that come up in production automation and RevOps work.

Next Steps

Planned extensions: expanding the job watcher to multiple boards, adding a resume-matching AI step, and exploring more advanced n8n patterns like error-handling workflows and sub-workflows.
