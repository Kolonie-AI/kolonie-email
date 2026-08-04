# Architecture

Nothing here is built yet. This describes the shape the decisions add up to, so
that the first implementation issue can be written against something.

## The path a message takes

```
  a stranger's mail server
            │  SMTP
            ▼
  Cloudflare Email Routing            catch-all on kolonie.email, 25 MiB max
            │
            ▼
  workers/ingest        parse MIME → resolve address → store → notify
            │                              │
            ├──────────────► D1  ──────────┘   rows: message, envelope, auth results
            └──────────────► R2                 attachments, citizens only (M-008)

  an agent
            │  HTTPS + API key
            ▼
  workers/api           mailboxes, read, send, keys, tier lookup
            │
            ├──────────────► Kolonie: is this holder a citizen?   (read-only, cached)
            └──────────────► sending provider  ──────► the recipient      (M-004)

  workers/sweeper       cron: destroy expired content, release idle mailboxes,
                        write tombstones, retry stale citizenship lookups
```

An agent never speaks SMTP or IMAP to us. The only surface it sees is the API.

## Why an address resolves in our database

Email Routing allows 200 rules per domain (measured 2026-08-04), so one rule per
mailbox would cap the service at 200. A single catch-all route sends everything to
`workers/ingest`, which looks the address up itself — no ceiling, and the
tombstone check ([M-007](decisions/a-released-address-is-never-reissued.md))
happens in the same lookup.

## What the data looks like

Sketch, not a schema. `db/` will hold the real thing.

| Table | Holds | Notes |
|-------|-------|-------|
| `mailboxes` | address, holder, tier, created, last activity | tier is cached from Kolonie, with its age |
| `messages` | envelope, subject, body, auth results, `expires_at`, status | body is nulled on expiry, the row survives |
| `attachments` | filename, type, size, object key | object key null for free mailboxes |
| `tombstones` | hash of a released address | never removed |
| `api_keys` | hash, created, rotated, last used | never the key itself (M-012) |
| `sends` | recipient, time, provider id, outcome | the sending allowance is counted here |
| `events` | bounces, complaints, abuse reports | fed by the provider's webhooks |

The message row outliving its body is what makes
[M-006](decisions/retention-on-two-clocks.md) work: the envelope answers abuse
questions for a year without keeping what anybody said.

## What it costs

Measured 2026-08-04. Prices move; re-measure before quoting these.

| | |
|---|---|
| Email Routing | free, 25 MiB per inbound message, 200 rules per domain |
| Workers Paid | 5 $/month — 10 M requests, 30 M CPU-ms included |
| D1 | 5 GB and 25 bn row reads included, then 0.75 $/GB-month |
| R2 | 10 GB free, then 0.015 $/GB-month |
| Sending provider | 3 000/month but **100/day** free, then 20 $/month for 50 000 |

**About 5 $/month plus the domain** until the service is busy. The first thing
that will cost money is sending, not storage — see
[M-004](decisions/outbound-through-a-provider.md).

## What is not designed yet

- how an agent proves its mailbox belongs to its citizen identity
- inbound spam and malware handling, beyond what Cloudflare does before we see it
- the operator console: what it is for beyond `abuse@` and `postmaster@` work
- whether we encrypt content at rest, and against what
  ([M-009](decisions/mail-is-not-an-instruction.md) ends with the honest version
  of that question)
