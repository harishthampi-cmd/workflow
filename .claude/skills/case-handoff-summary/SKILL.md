---
name: case-handoff-summary
description: Summarize Harish's open Salesforce cases (or all open cases for one account) as a plain-text list with case number, account, owner, issue and next step — for OOO handoffs, coverage notes, account reviews, or "what's on my plate". Use when Harish asks for "my open cases", "a handoff", "cases summary before I'm out", "open cases for <account>", or similar. Read-only.
---

# Case handoff summary

A list someone covering for Harish (or Harish's manager) can scan in a minute. Past feedback: "This looks like a terrible draft, I need this more clean", "I need ticket number, and ticket owner", "action items for us are wrong". So: plain text, one line per case, and next steps that are actually right.

Salesforce is read-only. Never send or post the summary.

## Step 1: Get the cases

Harish's open cases:

```sql
SELECT Id, CaseNumber, Subject, Status, Priority, Account.Name, Owner.Name, CreatedDate, Last_Significant_Update__c, Next_Planned_Communication_Date__c, People_ai_Product_Area__c
FROM Case WHERE OwnerId = '005Ri00000HxEJhIAN' AND IsClosed = false
ORDER BY Priority, CreatedDate
```

For an account review, swap the filter for `AccountId = '<id>' AND IsClosed = false` and keep `Owner.Name` — every row needs its owner.

If Harish gives a date range or says to exclude a case, apply it exactly.

## Step 2: Find the real state of each case

For each case, read the latest one or two EmailMessages (metadata first, then `TextBody` by `Id`). From them, work out: who's waiting on whom, and the actual next step.

Be careful with next steps — infer only what the emails support, and mark anything uncertain with "(confirm)". Don't assign customer-side work to Backstory, and don't invent work Backstory doesn't do (e.g. an integration that isn't in scope for that customer).

## Step 3: Write it

Plain text that pastes cleanly into Slack or email. No emoji, no bold, no link placeholders or brackets — Harish adds the case links. Group by status in this order, skipping empty groups:

```
Escalated
000XXXXX Acme Corp — forecast owner mismatch on Pipeline Changes. Waiting on Engineering reply in the Jira.

High priority
…

Waiting on customer
…

On hold
…
```

Each line: `CaseNumber Account (Owner if not Harish) — issue in a few words. next step.` One line per case. If Harish says a case will be finished today or should be dropped, remove it.

For an account review, add one honest line at the top on whether the cases are related or independent — only claim they're related with evidence.

## Step 4: Offer the format

After Harish is happy with the text, offer a .docx or a Slack-ready version if needed. Show the text first; build the file only on request.
