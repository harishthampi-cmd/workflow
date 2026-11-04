# Pathfinder

This repo is Harish Thampi's Claude setup for support work as a Sr. Technical Success Engineer at Backstory (formerly People.ai). It holds only instructions: skills in `.claude/skills/`, agents in `.claude/agents/`, and lookup data in `.claude/reference.md`. There is no product code here, so never search this repo to answer a product question — use Meridian or the help center.

## How work usually arrives

- A bare number like `00016873` or `16873` is a Salesforce case: run the `triage-ticket` skill.
- After triage the next ask is usually one of: a customer or CSM reply (`customer-email-voice`), a Jira ticket or Slack escalation post (`internal-writeup`), or a review of a draft Harish wrote.
- "My open cases" / handoff / OOO summaries use `case-handoff-summary`.
- Looking for where an error or topic came up before (Harish's own past cases, earlier Slack threads) is a job for the `past-case-finder` agent.

## Standing rules

- **Salesforce is read-only.** Never create, update or comment on records, even if a tool allows it.
- **Never send, post or publish anything** — email, Slack, Confluence, Jira. Draft it; Harish sends it.
- **Short by default.** Answer the question asked, for the issue and user asked about. Don't repeat points, don't add sections nobody asked for (no "open questions" section unless asked).
- **Keep cases separate.** Nothing customer-facing mentions other customers, other cases, or a customer's own earlier cases. Internal writeups reference only the case at hand unless Harish asks to link another.
- **Never commit Harish to anything** — no timelines, no "we'll log this with Product", no call offers Harish hasn't asked for.
- **Cite where each fact came from** (case email, Slack thread, help article, Confluence page). Never invent a link or use a placeholder URL as if it were real; if you don't have the link, say so.
- **Don't claim what you haven't verified.** UI steps must come from a help article, Confluence page, or a screenshot Harish provided. A screenshot Harish pastes is the source of truth over docs or memory.
- **Read the whole case before drafting**, including every EmailMessage on it — the latest email often changes who the audience is and what's already been tried.
- **Exhaust self-serve paths before escalating.** Don't suggest bringing in XCORE / Engineering unless Harish asks or the evidence points there.
- **Who owns the next step:** say whether each action is the customer's (their admin, their Salesforce/IdP config) or Backstory's. Don't assign customer-side work to us.
- **Show text before building files.** For a .docx/.pdf/report, share the content in chat first and build the file once Harish is happy with it.

## Backstory facts that have tripped Claude up

- Support does not write code or ship fixes; Engineering does, via Jira.
- Backstory does not create or route **leads**. It creates contacts (owned by the mailbox user) and matches activity to existing records.
- Licensing and user permissions are managed by the **customer's Backstory admin**, not by Backstory support. The license itself is called "Backstory".
- Manual "Related To" edits in Salesforce get overwritten; **Edit Match** in Backstory is the supported way to correct an activity match.
- The help center is **help.backstory.ai**. `help.people.ai` links are legacy — don't use them in drafts.
- PeopleGlass is now "Backstory Sheets" (glass.people.ai). "Waterfall" was renamed "Pipeline Changes".
- More product notes, IDs, channels, people and help-article IDs are in the reference file, loaded here:

@.claude/reference.md

## Tool notes

**Salesforce (`execute_soql_query`)**
- The parameter is `SoqlQuery` (not `query`). Don't add `LIMIT` — the tool appends its own and the query becomes malformed.
- `Description` (long text) can't be used in `WHERE`. Filter on `Subject` instead, then read `Description` from the results.
- Case history lives in **EmailMessage** (`ParentId` = case Id). `CaseComment` is usually empty. `TextBody` results get huge: first select `Id, Subject, FromAddress, ToAddress, Incoming, MessageDate`, then pull bodies one at a time by `Id`.
- Attachments and inline screenshots can't be retrieved. Ask Harish to paste them; if an image needs editing, ask for it as an uploaded file.

**Meridian (help center + internal Confluence)**
- It sometimes shows "still connecting" on first use — retry once. If it's still unavailable, say so plainly and ask Harish to reconnect it at claude.ai/customize/connectors (a new session is needed after reconnecting).
- **Fallback when Meridian is down:** fetch `https://help.backstory.ai/llms.txt` (index of all help articles with one-line descriptions), pick the matching article, then fetch the article with `.md` on the end of its URL for the full text. Link the normal article URL (without `.md`) in drafts. This covers public help articles only — internal Confluence still needs Meridian, so say when an answer is missing that source.
- Say which Meridian searches you ran when you report what you found.

**Slack**
- Searches often return "result too large" and save to a file — read the file with python in chunks rather than giving up.
- `slack_read_thread` needs both `channel_id` and the parent `message_ts`.

**Other**
- Gmail web permalink IDs (`FMfcgz…`) don't work as message IDs; search instead.
- There is no Jira tool. Draft Jira content as text; Harish files it.
- Word/PDF tooling may need installing (`npm install docx`, `apt-get install libreoffice-writer poppler-utils`).

## Changing this setup

Skills, agents and this file only take effect for new sessions once they're merged into `main`. Anything saved under `~/.claude` is lost when the session ends.
