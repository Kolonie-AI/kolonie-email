# AGENTS.md — kolonie-email

This file is binding for any agent working in this repository. Read it fully
before your first action. If it left you with a question you had to ask someone,
that is a defect in this file — open an issue for it before you continue.

Read [`STATUS.md`](STATUS.md) to find out where the work stands, and
[`MANIFEST.md`](MANIFEST.md) to find out why any of it exists. Both are short, and
every decision here is downstream of the second.

**`STATUS.md` is present tense and is rewritten, not appended to.** If you change
what is true — an account exists, a path works, a block clears — rewrite it in the
same commit. A status file that has to be read from the end is a diary, and the
next agent will not find the truth in it.

---

## 1. What you need

A GitHub token with **`repo` and `project` scope** and membership in the
`Kolonie-AI` organisation. Nothing else is required to *read* the work. Touching
the running service additionally needs the Cloudflare and mail-sending accounts,
which are **not** the ones Kolonie uses — see §5.

## 2. What this repository is

One repository holding the whole service: the code, its schema, its
configuration, its documentation. There is deliberately no second repository for
docs, infrastructure or the website. Kolonie is split across repositories because
`kolonie-docs` and `kolonie-infra` span several of them; nothing here spans
anything.

```
workers/api/       HTTP API: mailboxes, sending, keys, tiers, citizenship lookup
workers/ingest/    Email Worker on the catch-all route: parse, store, notify
workers/sweeper/   Cron: retention, mailbox recycling, tombstones
db/                D1 schema and migrations
web/               Landing page and the operator console
cli/  sdk/         Agent-facing tools
infra/             Wrangler config, DNS records, account setup — never secrets
docs/              Architecture, the Kolonie interface, decisions
tests/
```

Outbound sending lives in `workers/api/`, not in a worker of its own. It is a
request handler, not a daemon.

## 3. Where the work is

**On the existing Kolonie board**, not on a board of its own — project 1 of the
`Kolonie-AI` organisation, the same one `kolonie-platform` and `kolonie-docs`
use. Issues for this repository carry the label **`area:mail`**.

A board is a work queue, not a security boundary. What has to stay separate is
what can transmit damage — accounts, secrets, deployment — and a queue transmits
nothing. The same agents work both projects, and a second board would only mean a
second place to look. It also makes a `p1` here compete visibly with a `p1` on the
platform, which is a question worth seeing rather than splitting across two
brackets.

**Nothing adds an issue here to the board for you.** The project's five auto-add
workflows are all spent on older repositories and GitHub allows no sixth, so an
issue opened here is invisible until somebody adds it. Open it and add it in the
same breath:

```bash
gh project item-add 1 --owner Kolonie-AI \
  --url https://github.com/Kolonie-AI/kolonie-email/issues/<n>
```

Take the issue before you write code: move it to **In Progress** yourself. That
transition is the one thing nothing automates.

## 4. Decisions

Everything already settled is in [`docs/decisions.md`](docs/decisions.md) — a
register, and only a register. Where the argument is worth more than the verdict,
it is one file in [`docs/decisions/`](docs/decisions/) and the row links to it.

Decisions here are numbered **`M-0NN`**. They are not `D-` numbers: those belong
to `kolonie-platform`, agents collide on them, and a second numbering space in a
second repository would collide harder.

**Do not re-argue a decision that stands.** Reverse it if it is wrong — the
register keeps reversed rows and marks them, because the point is to stop the
question being reopened, not to be right in retrospect.

## 5. Red lines

**No secret enters this repository.** Not the DKIM private key, not the mail
provider's API key, not a Cloudflare token, not in a test fixture, not
base64-encoded, not "temporarily". The repository knows the *names* of secrets;
the values live in the deployment.

**This service does not use Kolonie's accounts.** Its own Cloudflare account, its
own sending account, its own domain. If you find yourself reaching for a Kolonie
credential to make something here work, stop — that is the failure this project is
shaped to prevent.

**Kolonie may not be made to depend on this service.** The interface runs one way
(§ [`docs/interface-kolonie.md`](docs/interface-kolonie.md)). A change that has
Kolonie calling into `kolonie.email`, or waiting on it, needs the maintainer.

**Receiving is never rate-limited or gated.** If a change makes an ordinary
incoming message fail to arrive for a non-citizen, it is the wrong change,
whatever it was meant to fix.

## 6. Writing

English, in issues, commits and documents, even when the conversation was in
another language. Conventional commits (`feat:`, `fix:`, `docs:`, `chore:`).

A measurement carries the date it was measured — prices, quotas and provider
limits change, and an undated number is a number nobody can check.

## 7. Confirm with the maintainer before

- registering or moving a domain, or changing DNS that affects mail delivery
- anything that spends money, including a plan upgrade
- publishing the service, or an announcement of it anywhere
- relaxing a limit that protects the sending reputation

## 8. When something here is wrong

Fix it in the same session you found it, or open an issue. A finding that lives
only in a chat transcript is gone when the session ends.
