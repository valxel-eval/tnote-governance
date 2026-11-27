# Product brief: Tiny Notes

## Problem
One person wants a single place to write short plain-text notes and read them back later. The request describes the solution ("add and list plain-text notes. Nothing else.") but not the problem behind it. We don't know:
- who this person is
- what they use for notes today
- what their current approach costs them in lost notes, time or switching between tools

Nothing yet shows that existing notes tools fail them. The problem is a hypothesis, and this brief asks the gate to confirm it.

## Users
- U1: A single individual, presumably the requester (unconfirmed): they need to capture short plain-text notes quickly and read back what they wrote, on one page, without organising, sharing or editing features getting in the way.

## Goals
- PG1: The user reliably saves notes when they use the app (measure: at least 1 note persisted in at least 80% of sessions, over the first 2 weeks after release)
- PG2: The user keeps coming back to the app (measure: the user saves notes in at least 3 separate weeks within the first month after release, confirmed with them at day 30)

## Evidence
- The product is a single-page app for one user that only adds and lists plain-text notes (E1)
- Success is defined as notes saved, stated as 1 note persisted per session (E2)
- The first year of features has a $60 budget (E3)
- The product starts under the general data class (E4)
- No source supports an unmet need that existing notes tools don't already serve (E5)
- Nothing confirms that the product records sessions and note saves. These events don't exist yet, so the goal can't be measured today (E6)

## Assumptions
- A1: We believe the user will save at least one note in a typical session; test: at least 1 note is persisted in at least 80% of sessions over the first 2 weeks after release. This first needs recorded "session started" and "note persisted" events.
- A2: We believe a deliberately minimal app (add and list only) is enough to keep the user coming back; test: the user saves notes in at least 3 separate weeks within the first month, confirmed in a short conversation at day 30.
- A3: We believe the product can be built within the $60 feature budget and run cheaply on shared platform defaults; test: feature spend stays at or below $60, and run cost, tracked monthly, stays at or below $5/month through month 3.
- A4: We believe notes will hold only general data, never PHI or payment data; test: the product owner confirms before G1 that notes are not meant for health or payment information.
- A5: We believe there is exactly one user at launch, the requester; test: the product owner names the user at G1, and the identity provider shows 1 active account at release.
- A6: We believe the user count stays at one for the first year, because the product is defined as single-user; test: active accounts remain at 1 at month 12. Any request to add a user goes back to the gate.
- A7: We believe usage is light, at most about 20 notes and 10 sessions a day, clustered in waking hours; test: daily note-persisted and session counts over the first 2 weeks stay at or below these figures.

## Non-goals
- Editing or deleting notes. The request says add and list only. Someone might expect these features, so the gap is flagged under Risks and Open.
- Search, tags, folders, pinning or any other way of organising notes.
- Rich text, attachments, images or markdown rendering.
- Sharing, collaboration or more than one user.
- A native mobile app. The product is one web page, and a mobile app would bring VX-18 obligations and cost.
- Offline-first sync across devices.

## Scope
- One web page where the signed-in user types a plain-text note and saves it.
- The same page lists the user's saved notes, newest first.
- Notes persist server-side through the product's BFF and the platform's event path (VX-16, VX-17). The build reuses catalog capabilities rather than building new ones (VX-6).
- Sign-in for the single user through the product's identity provider. The browser holds no tokens.
- Audit records for note creation, carrying ids and outcome but never note text (VX-15).
- SLOs, structured logs and traces (VX-8).
- Recording "session started" and "note persisted" events so PG1 and PG2 can be measured. This ships in the first slice, because without it neither goal can be checked.
- WCAG 2.1 AA for the page (VX-9).

## Scale
- launch: 1 user (A5)
- year one: 1 user (A6)
- busy hour: 3 times the daily average, over a 12-hour day (A7)
- where: one country, the requester's own (unconfirmed; to be named at G1)

## Availability
- tier: 3: if the app is down, one person can't add or read notes for a while. No one else and no other process depends on it. It starts at the pilot stage, and nothing justifies stepping up.

## Data class
- general: the product stores plain-text notes that one person writes for themselves, with no health or payment information intended (A4). Free text could still capture sensitive content, which is covered under Risks.

## Compliance
- WCAG 2.1 AA (via VX-9): the page must be keyboard-operable, labelled for screen readers and meet contrast requirements, verified by automated tests.
- Personal-data law of the user's country (for example GDPR if they are in the EU): notes and the sign-in account are the user's personal data. They must be stored in an allowed region and be exportable and erasable on request. This must be rechecked once "where" is confirmed. No sector regulation (health, payments) applies to general-class notes.

## Risks
- The platform path may cost more than $60 to build and a few dollars a month to run. That path is the BFF, identity, Kafka events, workers, and an audit topic kept six years with a write-once backup. Handling: reuse shared platform capabilities only, track cost monthly against A3, and escalate under VX-10 rather than overspend.
- The goal can't be measured today because no session or note-persisted events exist (E6). Handling: ship those events in the first slice, before any polish.
- The problem may not be real, since nothing shows existing tools fail the user (E5). Handling: confirm the user and their current tool at G1, and treat PG2 at day 30 as the keep-or-stop check.
- The user may type sensitive content into a note. With no delete, there is no way to remove it, and that also conflicts with an erasure request under personal-data law. Handling: state in the UI that notes are for general information, and have the gate decide whether delete belongs in scope (see Open).
- Note text lives in the Kafka topics that stores are rebuilt from (VX-16), so erasing a note means more than deleting a row. Handling: the design picks an erasure approach before build, such as per-note encryption keys that can be destroyed or compacted tombstones. Audit records hold ids only, so they need no change.
- "Session" is undefined, so PG1 could be gamed or misread. Handling: define it before build (proposed: a page load after 30 minutes of inactivity).

## Product
- id: TNOTE
- org: valxel-eval
- repo prefix: tnote-
- budget: 60
- run budget: pilot 5; growth 5; scale 10

## Agent notes
- Assumed: the single user is the requester, and they sign in. Server-side storage needs an identity under VX-17, even for one person.
- Assumed: a "session" is a page load after 30 minutes of inactivity. The request doesn't define it.
- Changed: I turned the goal measure "1 note persisted per session" into a threshold: at least 1 note in at least 80% of sessions over the first 2 weeks. As stated, the measure had no threshold or date, and a single empty session would fail it.
- Changed: I added PG2 (the user returns in 3 separate weeks in month one). The request gives only one goal, and that goal doesn't show whether the app is worth keeping.
- Changed: the only finding on the previous draft was that the critic's reply had no JSON object. That is a fault in the critic's output, not in the brief, and I can't fix it from here. The critic should be re-run. I left the brief's substance unchanged apart from one gap I found while revising: a risk that note text stays in replayable Kafka topics, which complicates erasure. If the critic's re-run raises real issues, they should come back as findings.
- Traded off: browser-only storage (localStorage) would be nearly free and would avoid the BFF, event and audit path. Against that:
  - notes would be lost when the browser is cleared
  - notes wouldn't follow the user between devices
  - "persisted" would be hard to measure

  I chose server-side storage on shared platform defaults, which conforms to VX-15, VX-16 and VX-17, at the risk of straining the $60 budget.
- Traded off: I set run budgets low (pilot and growth $5/month, scale $10/month) because the user count never grows. They are estimates until the platform's shared-infrastructure pricing is confirmed.
- Open: who exactly is the user, which country are they in, and what do they use for notes today?
- Open: is the $60 for build effort only, or does it also cover running costs? Will the gate accept a few dollars a month on top?
- Open: should a minimal delete be in scope despite "nothing else"? A note may capture something the user later wants removed, and erasure requests may require it.
- Open: is browser-only storage acceptable instead? It would cut cost sharply but give up durability and measurement.
- Open: is the id TNOTE confirmed, and which repos (beyond a single `tnote-` web app and BFF) are expected in valxel-eval?