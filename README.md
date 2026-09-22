# Codesk

**AI-native helpdesk for early-stage B2B SaaS.** One person runs customer
support for an entire company — Codesk drafts every reply, a human approves
it, nothing goes out without sign-off.

---

## The problem

Support at an early-stage SaaS company is repetitive in a way that's hard to
template: the same handful of question types, answered by digging through
your own codebase and docs every time. A generic AI chatbot can't do this —
it doesn't know your product. Codesk is built specifically to read *your*
codebase and docs, so the drafts it writes are grounded in what your product
actually does.

## How it works

1. **Connect your GitHub repo** (read-only OAuth) — Codesk ingests and chunks
   your codebase and docs into a searchable knowledge base.
2. **A customer emails support** — an inbound webhook turns it into a ticket
   automatically, no manual entry.
3. **Codesk drafts a reply** — the ticket is embedded, matched against your
   codebase via vector similarity search, classified by type, and handed to
   Claude to draft a reply grounded in cited source files and line ranges.
4. **A human reviews it** — the original email and the AI draft sit side by
   side. Approve, edit, or reject in one click. Every citation is visible and
   traceable back to the exact file/lines it came from.
5. **The reply sends** — only after approval. The ticket moves through a
   clear lifecycle (`open → pending_review → sent → resolved`) so nothing
   falls through the cracks.

**Nothing sends without a human in the loop. That's a permanent design
constraint, not a v1 limitation.**

## Under the hood

| Layer | Stack |
|---|---|
| Backend | NestJS · Prisma 6 · PostgreSQL 16 + pgvector |
| Frontend | Next.js 15 (App Router) · Tailwind |
| AI drafting | Claude Sonnet |
| Retrieval | OpenAI `text-embedding-3-small`, vector similarity search over ingested code + docs |
| Async processing | BullMQ + Redis |
| Email | Postmark (inbound parsing + outbound sending) |
| Auth | Server-side sessions (Postgres-backed, not JWT), per-tenant isolation enforced at the query layer |

Multi-tenant from the ground up — every table is scoped by tenant first, not
derived through joins, so one customer's data is never reachable from
another's session.

## See it in action

> 🎥 Demo videos coming soon. Planned:
> 1. **2-minute onboarding** — GitHub repo connected to first AI-drafted
>    reply, timed live.
> 2. **Draft review & approval** — the side-by-side review UI, citations,
>    and the 3-click-or-fewer approval flow.
> 3. **End-to-end** — a real inbound email triggering ticket creation, draft
>    generation, human approval, and the outbound reply landing in an inbox.

## Screenshots

> 📸 Coming soon — inbox view, draft review screen, settings/onboarding.

## Who it's for

- Founders doing their own customer support at 2–8 person teams
- Solo CS hires drowning in ticket volume at Series A companies

## Pricing

| Plan | Price |
|---|---|
| Starter | $149/mo |
| Growth | $399/mo |
| Scale | Custom |

## Status

In active development. Currently onboarding early customers before opening
up more broadly.

## Interested?

The codebase is private while we build with early customers — this repo is
just the front door. If you want early access, a demo, or want to talk about
how it's built, reach out:



**darshithagongle@gmail.com**
