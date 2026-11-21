---
name: call-follow-up
description: Use when a foodservice sales call, transcript, or note should become accurate First Bite records and a customer follow-up draft. Do not use for meeting preparation or general account research.
metadata:
  packs: contacts deals
---

# First Bite Call Follow-up

Turn a completed customer conversation into grounded CRM updates and a ready-to-review follow-up. First Bite is the system of record; supplied transcripts and connected tools provide evidence, not permission to invent data.

1. Read the supplied transcript, note, or recording summary. Extract the participants, company, products, decisions, commitments, unanswered questions, and next step. Mark uncertain facts as uncertain.
2. Inspect the available First Bite MCP tools before promising an action. Use contact search and record reads to resolve the company, relevant deal, and people. Never guess an id. If more than one record is plausible, ask the user to choose.
3. Read recent matching notes, opportunities, and sample requests when tools for them exist. Treat the recording link plus call date as the duplicate key for notes; treat company, recipient, and products as the duplicate check for a sample request.
4. Propose the record changes. Create or update records only through available First Bite tools and their approval flow. Preserve the customer's actual product names, location, recipient, and commitments. Do not infer stage, value, quantity, shipping address, or delivery date.
5. Draft the customer follow-up. Keep every customer-facing message as a draft—even when the user says “follow up today.” Do not call or offer a send action; sending is outside this skill. Include confirmed commitments and ask only for fulfillment details that are genuinely missing. Never claim a sample was requested, shipped, or delivered unless First Bite confirms it.
6. Report three things separately: what is now live in First Bite, what remains a draft for review, and what could not be completed because a capability or required fact is missing. Do not disguise a missing tool as completed work.

Use `list_contacts` and `get_contact` before `create_contact` or `update_contact`. Where deal tools are exposed, resolve the deal with `resolve_deal` and obey its `match` verdict rather than picking a row yourself. Filing the call's notes is its own skill, `call-notes`; prefer it over writing a note here, because it is idempotent on the call's source and this skill is not.

Do not simulate company-note, opportunity, sample, or email-draft writes with unrelated fields. A capability that does not exist is reported as missing, never approximated with one that does.
