# Architecture

```
 browser (Next.js)                     backend (Hono on Bun)               services
 ┌──────────────────────────┐  HTTP   ┌────────────────────────────┐
 │ agency and client views  │◄───────►│ routes    REST, auth, roles │──► Postgres (data + job queue)
 │ compose, calendar, inbox │         │ domain    pure rules        │──► object storage (media)
 │ analytics, approvals     │         │ adapters  one per platform  │──► social platform APIs (OAuth)
 └──────────────────────────┘         │ worker    tick-driven jobs  │──► AI model provider (optional)
        CLI (adsje) ─────────────────►└────────────────────────────┘
```

## Backend

- **routes** — one file per feature. Every client route sits behind a workspace guard: anonymous requests get 401 and
  non-members get 404. Roles (owner, admin, editor, approver, viewer) are checked in one place.
- **domain** — pure logic: analytics, queue slots, link tracking, plan limits, insight scoring and the learning loop.
- **adapters** — one module per platform behind a shared interface that declares limits (media count, video, threads)
  and optional capabilities (replies, metrics, comments). Adapters are tested against a fake HTTP layer.
- **worker** — the app runs on serverless hosting, so there is no resident process. Jobs live in a Postgres table and
  are claimed with row locking by a tick endpoint. Publishing resumes after a failure without repeating remote steps.

## Multi-tenancy and security

- Every product table carries a workspace id, and every query is scoped by it.
- Agencies own client workspaces; agency admins act on every client, while client users see only what they are invited to.
- Platform passwords are never stored. OAuth tokens are encrypted at rest and never sent to the browser.
- AI provider keys are encrypted and never returned by the API.
- Anti-spam guards: per-account daily caps, a duplicate-text guard, jittered schedules, and manual approval for Reddit.
- Public routes (approval links, redirects) are rate limited, with the counters shared through the database.

## Learning loop

Posts are tagged with an angle when published. A scheduled job scores each post against the workspace's recent
median, derives do and avoid rules, and a bandit picks what to try next. The learning lives in code and tables, not in
the model, and autopilot only ever creates drafts.

## Frontend

A Next.js App Router client using TanStack Query. Agencies land on a client list; each client has pages for compose,
calendar, queue slots, inbox, insights, analytics, reports, approvals, brand, accounts and members. Pure helpers have
unit tests; end-to-end specs drive the app in a real browser.
