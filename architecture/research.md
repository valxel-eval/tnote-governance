# Research: Tiny Notes

## Who has the problem
The request describes one user: a single person who wants to add and list plain-text notes on a single page. It doesn't say who that person is, how often they need to capture notes, or what it costs them today (lost notes, time, switching between tools). No evidence was found that the problem is real or costly. The research rests only on the request, and the request describes the solution rather than the problem.

## How they solve it today
- Not found: the request names no current alternative, workaround or competitor, and no search was run. Common alternatives such as built-in phone notes apps, text files, email-to-self or doing nothing are unsourced guesses, not findings.

## Market signals
- None found: the request gives no trend, regulation or market number, and no search was run.

## Evidence
- E1: The product is a single-page app for one user that only adds and lists plain-text notes ("Nothing else"). (confidence: high; source: the request)
- E2: The goal is notes saved, measured as 1 note persisted per session. (confidence: high; source: the request)
- E3: The budget for the first year of features is $60. (confidence: high; source: the request)
- E4: The product starts under the "general" data class. (confidence: high; source: the request)
- E5: There is an unmet need that existing notes tools don't already serve. (confidence: low; source: unsourced)
- E6: The product records sessions and note saves, so the goal measure can be computed. (confidence: low; source: unsourced)

## Assumptions
- A1: We believe the intended user will save at least one note in a typical session; test: at least 1 note is persisted in at least 80% of sessions over the first 2 weeks after release, which first needs recorded "session" and "note persisted" events that don't exist yet.
- A2: We believe a deliberately minimal app (add and list only) is enough to keep the user coming back; test: the user returns in at least 3 separate weeks within the first month, confirmed in a short conversation with them at day 30.
- A3: We believe the product can be built and run within $60 for the first year; test: actual feature and run cost, tracked monthly, stays at or below $5/month through month 3.
- A4: We believe notes will hold only general data, never PHI or payment data; test: the product owner confirms before G1 that the notes are not meant for health or payment information.

## Open questions
- Who exactly is the one user (is it the requester?), and what do they use for notes today?
- What makes existing notes tools inadequate for them, if anything? Without an answer there is no evidence the problem is real or costly.
- How is a "session" defined (page load, sign-in, a time window)? Is "1 note persisted per session" a floor, a target or an average?
- Where should notes persist (in the browser only, or in a server store), and does a single user still need sign-in? Under the platform constitution, any server-side change goes through a BFF, identity checks and the event and audit path (VX-15, VX-16, VX-17). That may strain a $60 budget.
- Does the "general" data class still hold if the user types sensitive content into free-text notes?
- Is the $60 meant for build effort, running costs, or both?
- Is the proposed id TNOTE confirmed, and which repos are expected in the valxel-eval org?

## Sources
- None: no search was run, as instructed. The only input was the request, which had no attachments or links.