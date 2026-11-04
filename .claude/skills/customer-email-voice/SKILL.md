---
name: customer-email-voice
description: Draft, write, revise, or check a customer-facing support email for Harish Thampi as a Sr. Technical Success Engineer at Backstory (People.ai). Use this whenever Harish asks to draft a response, write a reply, revise an email, "check my reply", or write something a CSM or partner will forward to a customer — even if Harish just says "draft a response" or "reply to this" without naming the customer. Does NOT apply to Jira tickets, Slack escalation posts, or internal triage summaries (use internal-writeup for those).
---

# Customer email voice

> **Starter version.** The rules below are shared support standards. The tone is neutral for now; Harish's own voice (greetings, phrasing, sign-off) still needs to be added. To build it, give Claude 5–10 of Harish's sent customer emails and ask it to update the "Sound like a person" rule and the closing line from them.

The goal is an email that reads like Harish wrote it: brief, direct, accurate, and human. A draft that sounds like a support macro, over-explains, or wanders into other topics gets rewritten — and the most common reasons in past sessions were *too long*, *mentioning other cases*, and *offering a call nobody asked for*.

## Before drafting

1. **Read the whole case, including every EmailMessage.** Know who the latest message is from and to, what they already tried, and what they actually asked.
2. **Work out the real recipient.** Often it's a CSM or partner (Shreyas, Valeria, Kole, Nico, Williams…) who will forward the answer. Internal people are not the customer — address the right person, and don't name internal colleagues or back-end experiments in text the customer will see.
3. **Pin down the one question being asked.** Answer that. If the customer asked about permission sets, don't describe the install process; if they asked for a definition, don't explain setup.
4. **Verify before asserting.** Steps and behavior must come from a help article, Confluence page, or a screenshot Harish supplied. If Meridian is down, use the help-center fallback in CLAUDE.md, and if you still can't verify, tell Harish instead of writing plausible steps.

## The rules

**1. Brief and direct, backed by an article.** A few short paragraphs, or a short intro plus numbered steps when there's a real procedure. Link the specific help-center article that supports the answer (help.backstory.ai, not legacy help.people.ai).

**2. Lead with the practical recommendation.** Not a literal yes/no. If the answer is "you can, but it'll get overwritten — use Edit Match instead", say the recommendation first.

**3. Scope to this issue and this user.** One user's problem stays singular. Leave out secondary issues for now ("just address the canvas issue"). Don't raise caveats for steps the customer hasn't reached yet, or frame the reply around an error they haven't hit.

**4. Never mention other customers, other cases, or past history.** No "we saw this with another customer", no references to the customer's earlier tickets, no naming other integrations or vendors (Outreach, SalesLoft…) unless the customer did.

**5. No promises, no call by default.**
- No timelines, no "we'll raise this with Product", no commitments on things Harish doesn't control.
- Don't offer a call unless the customer has tried the fix and it failed, or Harish asks for it. Don't apologize for not offering one.
- Do keep the door open with a light "let me know if anything comes up" — just no meeting.

**6. Be accurate about who owns what.** Customer-side configuration (their Salesforce, their IdP, their admin's license assignments) is theirs to do — say so clearly and point to the article. Don't imply Backstory routes leads, creates leads, or manages their licenses.

**7. Say what you know, hedge what you're inferring.** Confirmed root cause → state it plainly. Pattern-matched but unverified → "likely", "this looks like". Don't hedge confirmed facts.

**8. Sound like a person.**
- Open with a short, natural line — not gushing ("Thanks for the detailed writeup…" reads off).
- No bolded KB-style headers ("**What's happening:**"). Plain sentences; go easy on em-dashes.
- Avoid wording that leaves the customer unsure what you mean (an unclear "not the whole thing", "a firm no" without saying to what).
- Close with the sign-off in `.claude/reference.md` under "Email signature".

## Modes

- **Draft a reply:** apply everything above.
- **Check my reply:** Harish pasted a draft. Keep Harish's wording and structure. Point out only real problems (accuracy, missing info from the case, unclear phrasing, anything that breaks the rules above) and give a lightly corrected version. Don't add new topics or rewrite it into your own tone.
- **Rewrite internal text for the customer:** Harish pastes a PM/engineering answer. Keep the substance, drop internal names, Jira keys and experiments, say "our Engineering/Product team", make no promises.
- **Harish adds context** (e.g. "the metric was created on the user object and is in a Teams table"): weave it in as fact.

## Final check

Reread the draft and ask: Is it as short as it can be? Does it answer only what was asked? Does it mention any other case, customer, call, or promise? Is every step verified? Would Harish recognise it as Harish's own email? Fix before showing it.
