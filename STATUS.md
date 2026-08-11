# STATUS — what exists right now

Present tense only. **This file is rewritten, never appended to** — if it starts
reading like a diary, it has failed. Last rewritten 2026-08-11.

## In one line

The arrangement is written down, the domain is registered, **the project has begun
and nothing blocks the receiving path** — what does not exist yet is code.

## What exists

| | |
|---|---|
| This repository | Decisions, architecture, prior art, the Kolonie contract. No code |
| Its check | `bash .github/scripts/check.sh`, named in `AGENTS.md` §9 and run by `ci.yml` on every push and pull request. Four checks over the Markdown; grows with the code ([#4](https://github.com/Kolonie-AI/kolonie-email/issues/4)) |
| `kolonie.email` | Registered 2026-08-04, paid through 2028-08-04, privacy protection on. **A zone in Kolonie's own Cloudflare account, Free plan** (measured 2026-08-11) |
| Cloudflare access | Both development agents hold it, on that account ([M-015](docs/decisions/the-cloudflare-account-is-shared.md)) |
| `kolonie.sh`, `kolonie.to` | Registered the same day and **not part of this project** — the mail service gets one domain ([M-011](docs/decisions/the-domain-is-kolonie-email.md)) |
| Labels and board | `area:mail` and the usual set exist here; issues sit on the Kolonie board ([M-010](docs/decisions/issues-live-on-the-kolonie-board.md)) |
| A required status check on `main` | Since 2026-08-10 ([#5](https://github.com/Kolonie-AI/kolonie-email/issues/5)), so pull requests here auto-merge on green the way the older repositories' do |
| On the Kolonie side | A register row and a note in `kolonie-docs/state/decisions/kolonie-email-is-a-sister-project.md`; the repo table in `ARCHITECTURE.md` |

## What does not exist

No Email Routing rule, no Worker, no D1, no R2, no sending account, no DNS
records, no code, no website. The tier model, the retention rules and the API
shape exist **as decisions only** — nothing enforces them.

## What is blocked

**Nothing on the receiving path.** [#1](https://github.com/Kolonie-AI/kolonie-email/issues/1)
— its own Cloudflare account — is closed: on 2026-08-11 the maintainer reversed
that requirement rather than answering it
([M-015](docs/decisions/the-cloudflare-account-is-shared.md)). The account is
Kolonie's, the account-wide blast radius is accepted with a date on it, and
implementation may begin.

Two things are still a maintainer's, and neither blocks the receiving path:

| | |
|---|---|
| The sending account | Separate on purpose ([M-004](docs/decisions/outbound-through-a-provider.md)) and not yet created. It blocks outbound, which is not the first milestone |
| Money | Anything beyond the free tiers, including the day sending needs paying for (`AGENTS.md` §7) |

## What is decided

All of it is in [the register](docs/decisions.md), M-001 to M-015. The four that
shape everything else:

- separation runs through **accounts**, not through the work queue
  ([M-001](docs/decisions/separate-in-every-account.md)) — **with the Cloudflare
  account reversed out of it on 2026-08-11**
  ([M-015](docs/decisions/the-cloudflare-account-is-shared.md))
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
([M-011](docs/decisions/the-domain-is-kolonie-email.md)). The separate Cloudflare
account was required on 2026-08-04 and reversed on 2026-08-11, after it had held
every deployable piece of the project for a week
([M-015](docs/decisions/the-cloudflare-account-is-shared.md)).

## What happens next

1. **The receiving path**, which is the whole product: catch-all → ingest →
   readable over the API, with authentication results attached
2. **Tiers and the citizenship lookup**, against a stubbed Kolonie first
3. **The sweeper**, because retention that arrives after the data does is a
   migration rather than a policy
4. **The sending account**, when outbound is the milestone

Nothing in 1–4 is written as an issue yet. That is the next piece of work on this
repository.
