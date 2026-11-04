---
name: internal-writeup
description: Draft internal escalation text for a support case — a Jira bug, a Jira question for Product/Engineering, a comment on an existing Jira, a Jira title, or a short Slack post to a team channel (support-xcore, ama-forecasting, support-data and similar). Use whenever Harish asks for "a Jira", "a ticket for engineering", "a question for product", "a Slack post", "post this in <channel>", or "something I can send to <internal person>" about a case. Does NOT apply to customer-facing emails (use customer-email-voice) or to the triage summary itself.
---

# Internal writeup

Jira and Slack text that an engineer or PM can act on in under a minute. In past sessions the same corrections came back every time: "so long.. so so long", "I don't want to mix cases", "this sounds demanding", "they have to read the Jira for further details". So: short, one case, and phrased as a request.

Draft only — there's no Jira tool, and Harish files and posts everything. Never post to Slack.

## Rules for every writeup

- **Only this case.** No context from other customers or other tickets. If an existing Jira is clearly the same issue, one line is enough: "Looks related to FORECAST-194."
- **Only what the assignee needs to act.** Skip the troubleshooting story, side issues, and details they don't need (e.g. for a content transfer, just the user who inherits it).
- **Quote the customer verbatim** when describing the problem — don't paraphrase or "align" their words, because Engineering reads it literally.
- **State the cause neutrally.** Don't assume it's caused by a recent release or feature unless that's confirmed.
- **Include the OrgID** and the Salesforce case link (`https://peopleai.lightning.force.com/lightning/r/Case/<Id>/view`).
- **One piece of evidence**, the best one (one Zight video, one HAR file, one screenshot). Harish attaches it — leave a clear `[attach: …]` note.
- **No "Done when", no "Open questions" section,** no headers nobody uses.
- **UI steps must match Harish's screenshot** or a doc — don't invent steps to reproduce.
- **Always suggest a Jira title.**

## Jira — bug

```
Title: <component>: <what breaks, in plain words>

Project: <BUG / FORECAST / APEX…>   Priority: <…>   Component: <…>
Account: <name> (OrgID <id>)   Case: <link>

Description
<2–3 sentences: what the user sees, with the customer's words in quotes.>

Steps to reproduce
1. …
Expected: …
Actual: …

Evidence: [attach: …]

Ask: <one sentence — what you need from Engineering>
```

## Jira — question for Product/Engineering

Customer questions are asks, not demands: "Is it possible to…", "Can we confirm whether…".

```
Title: Question: <short question>

Account: <name> (OrgID <id>)   Case: <link>

Context
<1–2 sentences on what the customer wants and why, quoting them where useful.>

What we know
- <only facts that help answer the question, each with its source>

Questions
1. Is it possible to …?
2. …
```

## Jira — comment on an existing ticket

Two to four sentences: what's new (this customer, OrgID, case link), the customer's words, and any new evidence. No restating the original ticket.

## Slack post

2–4 sentences. It introduces the issue and points to the Jira — "they have to read the Jira for further details."

```
Hi team, <customer> (OrgID <id>) is seeing <issue in one line>. <one line on impact or what's been ruled out>. Details in <JIRA-KEY / [Jira link]>. Case: <link>
```

Harish adds the screenshot and Jira link. Pick the channel from `.claude/reference.md` (e.g. `#support-xcore` for escalations, `#ama-forecasting` for forecasting questions). Don't suggest an XCORE/Engineering escalation unless Harish has decided to escalate.

## Final check

Cut it in half if you can. Does it mention any other case or customer? Is every claim sourced? Does a question read as a polite ask? Is there a title?
