# Opportunity brief: Add and list notes (TNOTE-FEAT-1)

## Problem

The pain we think exists is losing short notes or having to hunt for them across tools. That is a hypothesis, and nothing here confirms it. We assume one person writes short plain-text notes, wants them kept, and wants to read them back later, newest first, without organising, sharing or editing features in the way.

The request describes a solution, not a problem. We don't know who the person is, what they use for notes today, or what that costs them in lost notes, time or tool switching. G1 is asked to confirm the problem. The one-sentence answer under Open (who U1 is and what they use today) would let us restate this section as the user's actual pain.

- **Evidence:** none. There is no support volume, incident, research or customer quote. The only source is the one-line request.
- **User (U1):** a single individual, presumably the requester. This is unconfirmed.
- **In scope:** type a plain-text note, save it, see all saved notes newest first, on one page.
- **Out of scope** (the request says "nothing else"): editing, deleting, search, tags, sharing, folders, rich text. Each would be a follow-up needing its own evidence. See Agent notes for how "deleting" interacts with TNOTE-3 erasure.

## Capability

New capability: nothing in the product covers note capture or listing. The only feature on record is this one (TNOTE-FEAT-1), there are no repositories yet, and no capability catalog entry was provided. The components named for it (tnote-web and tnote-notes-worker) are new.

## Product goal

- GOAL-TNOTE-1: the feature gives the user the one action the goal measures, saving a note, and the list lets them see that it was kept. The goal's measure is at least 1 note persisted in at least 80% of sessions over the first 2 weeks.

## Success metrics

- M1: In at least 80% of sessions, at least 1 note is persisted, over the first 2 weeks after release. This is GOAL-TNOTE-1's own measure. A session follows the 30-minute rule that tnote-web owns, and "persisted" means a `tnote.note.persisted.v1` event exists for it. It is measured from those events and session events, with no user input needed.
- M2: At least 99% of notes with a `tnote.note.created.v1` event have a matching `tnote.note.persisted.v1` event within 5 seconds of the created event's timestamp, and 0 notes have no persisted event 24 hours after creation. The population is all notes saved in the first 2 weeks after release. It is measured from timestamps already on the two events, with no extra instrumentation. It treats "persisted" as "listable", which holds if the list is read from the notes database that the worker writes (see Assumed). The thresholds are my proposal (see Assumed and Open).
- M3: The user has at least 1 persisted note in each of at least 3 separate weeks within the first 30 days after release. This is GOAL-TNOTE-2's measure. It is counted from `tnote.note.persisted.v1` events. GOAL-TNOTE-2 also asks for the user's confirmation at day 30, which depends on the user being reachable (see Assumed). This feature reports M3 but does not claim to move it (see Traded off).
- M4: At day 30, the single user is asked whether they still keep short plain-text notes in any other tool. Success is the answer "no other tool". The population is that 1 user, so the result is yes or no. It is the only check on whether the hypothesised problem was real. If the user can't be reached, M4 is reported as "not measured", never as a pass.

## Constitution articles

- VX-1
- VX-2
- VX-3
- VX-4
- VX-5
- VX-6
- VX-8
- VX-9
- VX-10
- VX-11
- VX-15
- VX-16
- VX-17
- VX-19
- VX-20
- TNOTE-1
- TNOTE-2
- TNOTE-3

## Agent notes

- Assumed: U1 is the requester, a single person. If other users exist, every metric here changes.
- Assumed: Today the user keeps notes somewhere else, or loses them. Nothing shows this.
- Assumed: Volume is small, so a single unpaginated list is fine. No limit on note length or count was given.
- Assumed: The capability catalog has no relevant entry, because none was provided. VX-6 should be confirmed against the real catalog at G1.
- Assumed: The 99% and 5-second thresholds in M2 are my proposals. Neither the request nor the goals give them.
- Assumed: The list is read from the notes database that tnote-notes-worker writes, so a note is listable once its `tnote.note.persisted.v1` event exists. M2 relies on this and needs no list-read instrumentation. If the design reads the list another way, M2 needs a list-read signal added.
- Assumed: Building this is small, but I have not estimated it and no per-feature budget figure was provided (VX-10). The only basis is the architecture's scope: two components (tnote-web and tnote-notes-worker), one event type plus its persisted counterpart, an audit topic and sign-in. G4 should confirm the cost and budget.
- Assumed: TNOTE-3 export and erasure on request can be met by an operator-run process rather than a UI. The request excludes delete and export features, and the one-page scope depends on this reading being acceptable.
- Assumed: Sign-in, the 30-minute session rule and the audit record per add are in scope despite "nothing else". The architecture already includes them, and they are needed to identify one user (VX-17) and to meet VX-15. This is my interpretation of the request.
- Assumed: The user can be reached and will answer at day 30. M3's confirmation and M4 both depend on this. If it fails, M4 is "not measured" and M3 rests on event counts alone.
- Assumed: The user will name the notes tool they use today when asked before G1. M4 compares against that answer. If they name none, M4 can only ask whether they now keep notes elsewhere.
- Assumed: VX-19 applies only if the design uses a managed cloud service from the platform catalog for the notes database or Kafka. The storage choice and region are not yet decided.
- Assumed: VX-3 and VX-20 apply to how this feature is delivered, not to what it does at runtime. No agent holds production credentials or promotes to production, and any access to a customer's cloud goes through the customer-granted roles in VX-20. I listed them because they are platform-wide.
- Changed: I added M2 and M4, which the brief did not ask for. The architecture saves through an event and a worker, so a saved note can briefly be missing from the list. M2 tests the reliability that GOAL-TNOTE-1 depends on. M4 tests the problem hypothesis itself, since M1 and M3 only measure use.
- Changed: The human feedback was an infrastructure fix (critic verdict truncation) and contained no instruction. The content changes in this revision come from the evaluator findings.
- Traded off: I named only GOAL-TNOTE-1 as the goal this feature moves. GOAL-TNOTE-2 (return use) depends on whether the need is real, which this feature cannot create. M3 keeps it measured without claiming it.
- Traded off: Alternative considered: the user keeps using their current notes tool, or does nothing. I prefer building only if that approach is failing them, and no evidence shows it is. I propose testing the hypothesis with M4 after release rather than blocking G1 on research, which rests on the unestimated cost assumed above. If G4 finds the cost is higher, G1 should instead require the one-sentence answer first (see Open).
- Traded off: "Nothing else" conflicts with the architecture, which already includes sign-in, a 30-minute session rule and an audit record per add. I kept them because VX-15 and VX-17 require them, and I added nothing else.
- Open: Who is U1, and what do they use for notes today? The answer decides whether the problem is real. Could the requester answer in one sentence before G1?
- Open: In which country or region is the user? TNOTE-3 says to recheck allowed storage region once this is confirmed.
- Open: Do you accept the M2 thresholds (99% persisted within 5 seconds, 0 without a persisted event after 24 hours)? Should read-only sessions count against M1, which would penalise sessions where the user only reads?
- Open: Is a maximum note length or count needed? An empty note should presumably be rejected.
- Open: Which constitution version should be recorded for this artifact? None was stated in the context given to me, and VX-2 requires one. G1 should not pass this artifact until it is recorded.
- Open: Is an operator-run export and erasure process acceptable under TNOTE-3, or does the gate want it as user-facing scope? This matters for design. Note text will sit in `tnote.note.created.v1` events that must stay replayable (VX-16). Audit records are append-only for six years (VX-15), though they hold no field values. The HLD must show how erasure works against both.
- Open: What is the per-feature budget under VX-10, so the build-cost assumption can be checked at G4?

## Provenance

- Artifact ID: TNOTE-FEAT-1 (opportunity brief).
- Parents: the one-line request for this feature; GOAL-TNOTE-1 in roadmap/goals.yaml; the architecture record valxel-eval/tnote-governance@c38385d.
- Constitution version: not recorded. The platform constitution and the Tiny Notes starter-draft constitution were given to me without a version identifier, and I won't invent one. VX-2 requires it, so this is a blocker for G1 (see Open). Someone with the real version should fill it in before the gate passes.
