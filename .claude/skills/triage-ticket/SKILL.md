---
name: triage-ticket
description: Triage a support case/ticket by its case number (e.g. "00016288", "triage ticket 00016288", "look into case 16288"). Pulls the case and its full email thread from Salesforce, checks Harish's past cases and related cases, researches the product behavior via Meridian (or the help center when Meridian is down), searches Slack, then gives a short summary with a likely diagnosis and solution path. Use this whenever the user pastes what looks like a Salesforce case number on its own, or asks to triage/investigate/look into a ticket or case — even without the word "triage." Do not use it for general Salesforce record lookups unrelated to case investigation, or for tickets in a different system (Jira, Zendesk, GitHub issues) unless the user explicitly says the number is a Salesforce case.
---

# Triage Ticket

Investigate a Salesforce support case and hand back a short summary that gets Harish to a **solution path**: what the customer reported, the most likely cause, what to do about it, and how to confirm. "I need solutions not only context from other tickets."

The output is a triage summary, not a finished deliverable. Don't draft a Jira, Slack post or customer reply until Harish asks.

Salesforce is read-only — never update the case. See CLAUDE.md for SOQL rules (`SoqlQuery` parameter, no `LIMIT`, no `Description` in `WHERE`).

## Step 1: Read the case and its full thread

```sql
SELECT Id, CaseNumber, Subject, Description, Status, Priority, Origin, CreatedDate, ContactEmail, Owner.Name, AccountId, Account.Name, People_ai_Product_Area__c, Prior_Case__c
FROM Case WHERE CaseNumber = '<number>'
```

If a short number like "16288" returns nothing, retry zero-padded to 8 digits ("00016288").

Then read the email thread — this is where the real history is (CaseComment is usually empty):

```sql
SELECT Id, Subject, FromAddress, ToAddress, Incoming, MessageDate
FROM EmailMessage WHERE ParentId = '<case_id>' ORDER BY MessageDate ASC
```

Pull `TextBody` for the messages that matter one `Id` at a time (bodies are large). Note who's on the thread (customer vs CSM/partner), what's been tried, and the latest ask. If `Prior_Case__c` is set or the thread looks like a continuation, read the earlier case too.

Note the OrgID if it appears in the case or emails, and the `CreatedDate` (anchor for release timing in step 4).

**If Harish pasted a screenshot, treat it as the primary evidence.** Read the exact error text from it and search on that; it outranks your reading of the description. Attachments on the case can't be fetched — if one matters, ask Harish to paste it.

## Step 2: Search Harish's own past cases first

Harish has often solved the same thing before ("I have worked on this issue in the past… dig into my past cases"). Search cases owned by Harish (`OwnerId = '005Ri00000HxEJhIAN'`) on the error text and feature keywords, any status:

```sql
SELECT Id, CaseNumber, Subject, Status, Account.Name, CreatedDate, ClosedDate
FROM Case WHERE OwnerId = '005Ri00000HxEJhIAN' AND (Subject LIKE '%<keyword>%' OR Subject LIKE '%<keyword2>%')
ORDER BY CreatedDate DESC
```

For a promising closed case, read its last few EmailMessages to see how it was resolved. For a broad hunt across Salesforce and Slack, hand it to the `past-case-finder` agent.

## Step 3: Related cases — same account and elsewhere

- **Same account:** `WHERE AccountId = '<account_id>' AND Id != '<case_id>' ORDER BY CreatedDate DESC`. Skip if the case is on the placeholder "Backstory" account.
- **Same issue elsewhere:** 2–3 targeted `Subject LIKE` searches (feature name, UI element, error text). Never filter on `Description`.

Keep only cases that directly match this issue — "be very specific in your search and remove unrelated cases." A closed match is worth more than an open one: find out what resolved it.

## Step 4: Research the product behavior

Search Meridian for the feature area using the customer's words and the product terms involved. Check release notes if the case was opened around a release. When a specific article or Confluence page matters, read the full source rather than relying on the snippet.

**If Meridian is unavailable** (retry once if it's "still connecting"): fetch `https://help.backstory.ai/llms.txt`, pick the matching articles, and fetch each with `.md` appended for full text. Tell Harish that Meridian was down and that internal Confluence wasn't checked.

Note which searches you ran — Harish will ask.

## Step 5: Search Slack

`mcp__Slack__slack_search_public_and_private` for: the customer/account name plus feature keywords (`#customer-<name>`), the feature or error text alone, and the case number. If the result is too large, read the saved file in chunks.

## Step 6: Write the summary

Keep it short. Three parts:

1. **What the customer reported** — facts only: account, OrgID if known, who, what, when, status, what's been tried. One or two sentences.
2. **Likely cause and solution path** — your best diagnosis, labeled by confidence:
   - *Confirmed* only with direct evidence (a matching resolved case's notes, an engineer's statement, a doc that describes exactly this, the customer's screenshot matching a documented error state).
   - *Likely* when it's an inference — and then list the one or two checks that would confirm or rule it out.
   - Give the fix or next step, and **say who owns each action** — the customer (their admin, their Salesforce/IdP config) or us. Don't make customer-side work ours.
   - Prefer self-serve fixes from the help center. Don't propose escalating to XCORE/Engineering unless the evidence says it's a bug or Harish asks.
3. **Supporting evidence** — two or three bullets max, each with its source: the matching past case (number + how it was resolved), the help article (linked), the Slack thread. Drop anything that doesn't directly bear on this issue.

Cite help articles with their help.backstory.ai URL. Cite cases by number with a one-line note. Never use a placeholder link.

## Step 7: Offer next steps

Ask what Harish wants next — usually a customer/CSM reply (`customer-email-voice`), a Jira or Slack post (`internal-writeup`), or nothing. Ask any real open question directly in the reply rather than as a formatted section.
