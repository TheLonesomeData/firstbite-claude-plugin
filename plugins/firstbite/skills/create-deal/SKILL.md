---
name: create-deal
description: Use when an operator has shown real interest (an interested campaign reply, a request for pricing or a sample, a meeting that went well) and the user wants it in the pipeline, or when a deal should move stage after a sample or a pricing talk. Creates one deal per product, checks for existing deals first, and pairs each deal with a follow-up reminder. Do not use for cold prospects or for bulk-loading a list of companies; those belong in campaigns and tags.
metadata:
  packs: deals catalog contacts reminders
  source: First Bite customer success deal-creation best practices
---

# Create a deal

A deal is an engaged lead for one product. The pipeline is only useful while that stays true: workspaces that filled it with every prospect ended up with thousands of silent deals and a cleanup session. Follow these steps in order.

## 1. Decide whether this should be a deal at all

- Create a deal for an operator who has **engaged**: replied with interest, asked for pricing or a sample, or took a meeting. Replies and campaign stats already track everyone else.
- If the user asks to add prospects who have not responded, or a whole list, say that First Bite's practice is to keep them in campaigns and tags, and ask whether any of them have actually engaged. Create deals only for the ones who have.

## 2. Know the operator before writing

- Resolve the operator with `resolve_company` and obey its verdict. Several matches means ask; none means say so. Never create a company just so a deal has somewhere to go.
- Look at what already exists: `resolve_deal` with the operator's name lists its deals. If a deal for the same product is already there, update that one; do not add a second. `create_deal` also returns the existing deal rather than a duplicate, and says so in `created: false`. Only pass `allow_duplicate` when the user explicitly wants a second deal for the same product.
- If the user is about to reply to the operator, a quick read of the company (`get_company`, recent notes) keeps the reply relevant. Say what you found in a sentence; do not recite the record.

## 3. One deal per product

- Each product the operator is interested in is **its own deal**. Two products means two `create_deal` calls. This is deliberate: churn is tracked per product.
- Find each product with `find_products` and use the product line the operator asked about, with a variant when they named a size or pack.
- If the right product is not in the catalog, do not substitute a similar one. Tell the user to add a placeholder in **Workspace > Products** with a best-estimate price and churn rate (both are required), then create the deal against it. The deal value recalculates when the product is corrected later. No tool here adds a product.

## 4. Fill in the deal

- **Owner:** the user, unless they name a teammate (`account_owner_name`).
- **Stage:** the workspace's own early stage. Stages are named per workspace; read them with `list_deal_stages` and never invent one. The usual shape is a qualified-lead stage, then an active or sampling stage, then pricing or negotiation, then closed won or lost.
- **Value:** leave volume and revenue empty so First Bite forecasts them from the product price and the operator's sales rate. Set an override only when the user gives a number, in the account's volume unit (`volume_unit` on any deal result, usually lbs): convert cases with the pack size from `find_products`, or ask, and send `volume_unit` with it. When an operator buys at more than one price, the forecast uses the product's single price; mention that the more conservative price is the practice if the user asks.
- **Close date:** only when the user gives one.
- Show the user the deals you are about to create (operator, product, owner, stage) in one short list; the approval step does the rest.

## 5. Pair every deal with a reminder

- After creating a deal, offer a follow-up reminder with `create_reminder`, and create it if the user agrees or already asked. This is the step customers forget most often.
- After a sample ships, a check-in reminder is the default: ask when, and suggest about a week after the expected delivery if they have no date.

## 6. Moving a deal forward

- When the user says a sample went out, move the deal to the workspace's sampling stage. Nearly every workspace has one under its own name ("Sample Sent", "Testing", "Active Engagement", "Sampled"). If exactly one stage clearly means "sample sent", use it and say which; if none or several could, list the real stages and ask.
- `list_deal_stages` gives each stage's win rate, which says how far along it is (the display order does not). If the user told you which stage to use, move the deal even when that is backwards (a lower win rate), and mention it. If you chose the stage yourself and it is backwards, for example a sampling stage for a deal already in pricing, ask first.
- Some stages ask for a capture form in the app. If `update_deal` refuses a stage for that reason, tell the user to make that move on the deal page.
- When the user reviews many deals at once, suggest the Board view in Deals, where they can drag cards between stages.

## What this skill cannot do here

Say these plainly when they come up, and point to where the user does them in First Bite:

- Linking the campaign reply thread to the deal, and "Assign to deal" on an email: done from the reply or the email in the app.
- Replying from the user's work email ("Take over" on a reply): done in the campaign's Replies tab.
- Company status tags such as "Current Customer": done on the company page.
- Do Not Contact belongs on the contact, not a tag: `update_contact` with `is_do_not_contact` sets it, and it removes them from campaigns.

## Report back

End with what now exists (each deal with its id, product, stage and owner, and any reminder), what was already there and left alone, and what the user still has to do in the app.
