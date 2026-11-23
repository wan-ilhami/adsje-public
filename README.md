# adsje

**One control panel for every client's social posts.**

adsje is a multi-client social posting panel for agencies and solo operators. An agency owns many client workspaces.
Each workspace connects its own social accounts through the official OAuth flows, then composes, schedules, approves,
publishes, and reads replies and metrics in one place. It never stores a platform password.

## Screenshots

Screenshots are to come.

## What's inside

- **Client workspaces** — an agency with owner, admin and staff roles manages many clients. Each client workspace has
  its own accounts, brand kit, queue and reports. A solo mode lands straight in the only client.
- **Platforms** — Threads, Instagram, Reddit, Bluesky, LinkedIn, Facebook and X, each behind one adapter interface.
  A platform with no app credentials simply stays switched off.
- **Compose and schedule** — text, image carousels, video, threads and a first comment. Posts go out now, at a set
  time, or into the next free slot of a per-account queue.
- **Approval** — editors draft, approvers sign off, and clients can approve from a link without an account.
- **Inbox and analytics** — replies and metrics are synced on a schedule. Reply suggestions are drafted for you to
  edit and send, and tracked links show what drives clicks.
- **AI assist** — caption and reply drafts, product fact sheets learned from a website, and a learning loop that
  scores past posts and suggests what to try next. Drafts are never published without a person.
- **Operations** — weekly reports, audit log, notifications, export, and optional Stripe billing with plan limits.
- **CLI** — an `adsje` terminal client over the same REST API.

## Architecture

See [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## What this repository is

This is the **public** showcase of adsje. The application code lives in a private repository. This repo holds
only the product description, the architecture overview, the version table and screenshots — no source code, keys,
tokens, internal URLs or personal data.
