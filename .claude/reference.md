# Reference data

Lookup facts gathered from past sessions. Verify anything that looks stale against Salesforce, Slack or the help center before relying on it in a customer-facing draft, and update this file when something changes.

## Salesforce

- Harish's User Id: `005Ri00000HxEJhIAN`
- Case link format: `https://peopleai.lightning.force.com/lightning/r/Case/<CaseId>/view`
- Unassigned cases sit in the **"Service Case"** queue.
- Internal-origin cases can sit on the placeholder **"Backstory"** account (`001Ri00000tsctbIAA`) — they have no real customer account.
- Replies on a closed email thread open a new case. When a case looks like a continuation, check `Prior_Case__c` and earlier cases from the same contact.
- Useful Case fields:
  - `People_ai_Product_Area__c` — product area (e.g. `GDPR Compliance` for privacy requests, `Data Pipeline`, `Foundations`)
  - `Last_Significant_Update__c`, `Next_Planned_Communication_Date__c`, `Days_Since_Last_Touch_c__c`
  - `Escalation_Comments_Notes__c` (often empty), `Prior_Case__c`
  - `Service_Portal_Primary_Case_Category__c`, `Service_Portal_Primary_Case_Sub_Category__c` (often blank)
  - Standard: `Reason`, `Type`, `Comments`
- The Jira link shown in the case's right-hand panel is **not** reachable through SOQL (`Jira_Link__c`, `Jira_Ticket__c`, `Jira_Issue_Key__c` don't exist). Ask Harish if a Jira exists.

## Customers (OrgID / Salesforce Account Id)

| Customer | Backstory OrgID | Account Id |
|---|---|---|
| Iron Mountain | 200000303 | 0014100000bROeeAAG |
| NVIDIA | 200000429 | 00141000006UZN2AAO |
| IBM | — | 00141000004WScMAAW |
| Tanium | — | 00141000004WSh1AAG |
| Presidio | 200000653 | 0014100000UgXloAAF |
| OpenAI | 200000627 | — |
| Backbase | 200000611 | — |
| SailPoint | 200000155 | — |
| GXO (sandbox) | 200000694 | — |
| DHL | 200000683 | — |
| Dozuki | 200000655 | — |
| TransUnion | 200000552 | — |
| Anaplan | 200000367 | — |
| Mapbox | 100000083 | — |

## Slack

- Customer channels: `#customer-<name>` (e.g. `#customer-ibm`, `#customer-nvidia`, `#customer-hpi`, `#customer-flexera`, `#customer-mapbox`, `#customer-dhl`). Red Hat also uses `#hierarchy-coverage-rh`; IBM also `#ibmcio-peopleai-team`.
- Escalation / support: `#support-xcore` (XCORE escalations), `#support-data`, `#support-peopleglass`, `#support-salesai`, `#support-apps`, `#support-updates` (Salesforce bot case feed)
- Team: `#technical-success`, `#csg-all`
- Ask the experts: `#ama-product`, `#ama-forecasting`, `#ama-salesai`, `#ama-engagement-dashboards`, `#ama-pai-bot`, `#ama-demo-org`
- Other: `#releases`, `#qa`, `#mcp-core`, `#proj-public-api-documentation`, `#eng-matching-initiative`, `#eng-int-foundation`
- Harish's Slack user Id: `U0C76U8TUKU`

## Jira

No Jira tool is connected — drafts only. Projects seen: `BUG` (bug triage), `FORECAST`, `APEX`, `ENG`, `DOCR`. Component used for EDB issues: "EDB & Metrics".

## People (for internal drafts)

- CSMs: Shreyas Gore, Nico Alvarez, Kole Franklin, Valeria Pacheco, AJ Balsamo, Jeff Palin, Jen Grange, Amber Wilson (Lead CSM)
- Professional Services: Kaisha Fox (Principal Consultant), Justin Monell (implementation consultant)
- Product: Danny Lai (Activity Sources, visibility), Ryan Pollack (Forecasting), Bert Yagrich (PeopleGlass content transfer), Jacob Frey and David Maksymiuk (My Meetings), David Toca Avila
- Engineering: Vadym Rudenko (matching logs), Alex Oleksenko, Jacob Younan (MCP)
- Support / TS teammates: Danish Adeel, David Depman, Duvonte Hunt, Andrew Fearon, Williams Santana
- Teams: XCORE (escalations), PeopleGraph (people/title data)

## Help center (help.backstory.ai/en/articles/…)

| Topic | Article Id |
|---|---|
| Single Sign On | 15252780 |
| Okta SAML | 15252837 |
| Microsoft 365 Integration | 15252844 |
| Teams | 15252829 |
| Salesforce Integration | 15252924 |
| Integration User Permissions | 15252897 |
| Managed Package | 15252916 |
| Fields Backstory updates | 15252864 |
| Edit captured activities | 15252765 |
| CRM Activity Sources | 15252845 |
| Dynamics | 15324890 |
| My Meetings | 16397792 |
| New Product Releases | 15252920 |
| CRM Push Exclusions | 15252715 |
| Email and Calendar Exclusions | 15252887 |
| Engagement Metrics | 15252777 |
| MCP | 15138623 |
| Query API | 13621758 |
| Post-outage re-authentication | 16977252 |

Full index: `https://help.backstory.ai/llms.txt`.

Confluence pages (via Meridian) that have come up often: "People.ai in Salesforce - Error States", "Troubleshooting: Authentication Issues", "EDB Data Visibility using Object Permissions", "Custom Persona Title Classification", "Who is who: Engineering teams at People.ai", "Enabling SalesAI: A Technical Guide" (template for TS articles).

## Product notes

- **SSO / SAML:** SP Entity ID is fixed at `https://app.people.ai`; ACS path is `/saml/sso`. The signing certificate is carried inside the IdP metadata, and the metadata is re-read when the SSO config is saved.
- **MCP tools** are enabled per org in superadmin `mcp_enabled_tools` (`app.people.ai/superadmin/org-config/`); changes apply within about 6 hours.
- **Signals** take 24–72 hours to populate.
- **Engagement scoring** uses captured activity, not activity synced to Salesforce.
- **Who-is-Who:** one WhoId per activity; other participants are on EventRelation/TaskRelation. Recalcs use the current CRM snapshot.
- **Cadence-tool dedup** (Outreach, Apollo etc.) needs a Message-ID from the sending tool.
- **Title classification:** a classifier config upload takes 1–2 weeks and is a broad change; one-off contact fixes apply immediately.
- **Personal email domains** list: `ppl_email/domains/personal.txt`. Email notifications go through customer.io.
- **URLs:** web app `app.people.ai`; admin panel `edb.backstory.ai`; Sheets/Forecast `glass.people.ai`.
- **PeopleGlass in Salesforce:** the connected app is managed under External Client App Manager → PeopleGlass Client. A permission set alone has failed before — also check Manage Profiles.

## Email signature

Harish's own closing line hasn't been set yet — until it is, close with "Best regards,". If a full signature is needed (confirm the title wording with Harish):

```
Harish Thampi
Sr. Technical Success Engineer - Backstory.ai
```
