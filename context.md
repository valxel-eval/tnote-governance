# Tiny Notes

One person wants a single place to write short plain-text notes and read them back later. The request describes the solution ("add and list plain-text notes. Nothing else.") but not the problem behind it. We don't know:
- who this person is
- what they use for notes today
- what their current approach costs them in lost notes, time or switching between tools

Nothing yet shows that existing notes tools fail them. The problem is a hypothesis, and this brief asks the gate to confirm it.

## Users

- U1: A single individual, presumably the requester (unconfirmed): they need to capture short plain-text notes quickly and read back what they wrote, on one page, without organising, sharing or editing features getting in the way.

## Components

- tnote-web (bff): serves the single page and the notes API behind it; owns sign-in and the 30-minute session rule, publishes one tnote.note.created.v1 event plus one audit record per add, and reads the newest-first list from the notes database (S1, S2, S3, S4, S5, S6, S7)
- tnote-notes-worker (event-worker): applies note and session events to the notes database once and in order, and publishes tnote.note.persisted.v1 (S1, S6)

The product architecture (architecture/product-architecture.md) is the reference for every feature's design; a design that departs from it proposes an amendment with an ADR.
