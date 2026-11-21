---
name: call-notes
description: Use when a finished customer call, transcript, or note-taker summary should be filed onto the right First Bite deal and company, exactly once. Do not use for drafting a customer reply, updating a deal's stage or value, or preparing for an upcoming meeting.
metadata:
  packs: deals contacts
---

# First Bite Call Notes

File what was said onto the record it belongs to, without writing it twice and without inventing the record. This is a narrow job on purpose: everything that follows a call — the reply, the sample, the stage change — is someone else's skill.

1. Read the supplied transcript, summary, or recording page. Pull out the operator, any deal the call names, the date, the participants, the recording link, and the note-taker's own id for the call if it has one. Keep the customer's wording for products and commitments; do not tidy it into ours.

2. Resolve the deal with `resolve_deal` before writing anything. Obey the `match` verdict rather than your own reading of the list:
   - `exact` — one deal matched; proceed.
   - `ambiguous` — several matched. File the company note now (step 4), then show the deal candidates with their operator, product, stage and owner, and ask which one also gets it. Do not pick the first, the most recent, or the one whose name looks closest.
   - `none`, or only closed deals — file the note on the company alone, and say that no open deal matched. Never create or reopen a deal so that a note has somewhere to go; offer it instead.

3. Do not resolve the deal with reporting or current-view tools. They read the projection behind the deals table, which lags the live records and does not move at all on some environments, so a deal created recently will read as missing. `resolve_deal` reads what you can actually write to.

4. Write the note with `create_note`, once on the deal (`target: "deal"`) and once on its company (`target: "company"`, `target_id` = the deal's `contact_company_id`). If the call was about one store rather than the account, use `target: "location"` with the door's id instead of the company.
   - `content` is **plain text** and must be words from the call or the user, never a summary you wrote to fill a gap. Never send HTML, Markdown tables, or `<br>`; it is escaped for you and markup will be shown to the user literally.
   - Always pass `source.system`, naming the note-taker in plain lowercase (`fathom`, `gong`).
   - Pass `source.external_id` whenever the upstream call has an id. This is what makes a re-import safe: with it, the same call filed again returns the existing note even if the summary was regenerated with different wording. Without it, only a byte-identical re-import is caught.
   - Pass `source.url` for the recording link so a reader can get back to the call.
   - If the user points at a summary you do not have ("file that summary"), ask for it. Do not compose one.

5. Read `created` in the response and report it honestly. `created: false` means the note was already there and nothing was written; say that, rather than telling the user you added a note. A re-run that writes nothing is the tool working, not a failure.

6. Confirm with `get_deal` if the user needs assurance the note landed. Do not assert success purely from the write's own response.

7. Say plainly what you could not do. If no capability exists for the deal's stage or for a follow-up reminder, name the gap. Do not approximate a missing capability with one that happens to be available: a note is not a stage change.

Nothing in this skill sends anything to a customer.
