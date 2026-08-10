# STATUS — what exists right now

Present tense only. **This file is rewritten, never appended to** — if it starts
reading like a diary, it has failed. Last rewritten 2026-08-10.

## In one line

The arrangement is written down, the domain is registered, nothing is built and
nothing can be deployed until [#1](https://github.com/Kolonie-AI/kolonie-email/issues/1)
is answered.

## What exists

| | |
|---|---|
| This repository | Decisions, architecture, prior art, the Kolonie contract. No code |
| Its check | `bash .github/scripts/check.sh`, named in `AGENTS.md` §9 and run by `ci.yml` on every push and pull request. Four checks over the Markdown; grows with the code ([#4](https://github.com/Kolonie-AI/kolonie-email/issues/4)) |
| `kolonie.email` | Registered 2026-08-04, paid through 2028-08-04, privacy protection on |
| `kolonie.sh`, `kolonie.to` | Registered the same day and **not part of this project** — the mail service gets one domain ([M-011](docs/decisions/the-domain-is-kolonie-email.md)) |
| Labels and board | `area:mail` and the usual set exist here; issues sit on the Kolonie board ([M-010](docs/decisions/issues-live-on-the-kolonie-board.md)) |
| On the Kolonie side | A register row and a note in `kolonie-docs/state/decisions/kolonie-email-is-a-sister-project.md`; the repo table in `ARCHITECTURE.md` |

## What does not exist

No Cloudflare account for this project, no zone in it, no Workers, no D1, no R2,
no sending account, no DNS records, no code, no website. The tier model, the
retention rules and the API shape exist **as decisions only** — nothing enforces
them.

**No required status check on `main`.** The check runs, and nothing makes a pull
request wait for it, so pull requests here still wait for a person rather than
auto-merging the way the four older repositories' do (`kolonie-docs`
`ARCHITECTURE.md`, *Merging*). That is a branch-protection setting and a
maintainer's to make.

## What is blocked

[#1](https://github.com/Kolonie-AI/kolonie-email/issues/1) — its own Cloudflare
account, or a token that reaches Kolonie's Workers. Blocked on a maintainer
action, deliberately deferred on 2026-08-04. Everything touching the deployment
waits on it; schema, API shape and tests against local stubs do not.

## What is decided

All of it is in [the register](docs/decisions.md), M-001 to M-014. The four that
shape everything else:

- separation runs through **accounts**, not through the work queue
  ([M-001](docs/decisions/separate-in-every-account.md))
- **no mail server of our own** — Cloudflare Email Routing, Workers, D1, R2
  ([M-003](docs/decisions/no-mail-server-of-our-own.md))
- **receiving is never rationed**; sending and the name are what citizenship buys
  ([M-005](docs/decisions/never-ration-receiving.md))
- **retention on two clocks**, content and mailbox, envelope outliving both
  ([M-006](docs/decisions/retention-on-two-clocks.md))

Four questions are open on purpose and are listed at the bottom of the register.

## How this got here

Brainstormed with the maintainer on 2026-08-04, in one session, starting from the
observation that agents look for an address rather than for a colony. Three
existing services were studied before anything was decided — see
[prior art](docs/prior-art.md), which also records what we deliberately do *not*
copy.

Two things were considered and dropped early: building on a VPS with Postfix
([M-003](docs/decisions/no-mail-server-of-our-own.md)), and giving the project its
own project board ([M-010](docs/decisions/issues-live-on-the-kolonie-board.md)).
A neutral brand without the Kolonie name was proposed and rejected
([M-011](docs/decisions/the-domain-is-kolonie-email.md)).

## What happens next

1. **#1 is answered** — Cloudflare account and token, sending account
2. **The receiving path**, which is the whole product: catch-all → ingest →
   readable over the API, with authentication results attached
3. **Tiers and the citizenship lookup**, against a stubbed Kolonie first
4. **The sweeper**, because retention that arrives after the data does is a
   migration rather than a policy

Nothing in 2–4 is written as an issue yet. That is the next piece of work on this
repository, and it does not need #1.
