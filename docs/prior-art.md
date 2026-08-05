# Prior art

Four services sell what we are about to give away, and one of them published its
source and its strategy. Read this before proposing anything; most obvious ideas
have already been tried by somebody in this table, and two of them were tried and
rejected for reasons worth knowing.

**The first three were measured on 2026-08-04**, and **OpenMail on 2026-08-05**,
from public documentation and source. Providers change their terms; re-measure
before quoting any of it. That warning is not decoration here: OpenMail's free
tier as described in `kolonie-email#2` on 2026-08-05 — *"100 sends a day, 3 GB"* —
is not on the pricing page the same day, and its Pro tier turned out to be €9/mo
**plus usage** rather than €9/mo.

## The four

### agentsmail.org — the one we learned the most from

MIT-licensed, source public at
[`huberthe-pro/agents-mail`](https://github.com/huberthe-pro/agents-mail), plus a
product strategy document in the repository. Cloudflare Workers, D1, R2, inbound
via Email Routing, outbound via Resend — the exact stack
[M-003](decisions/no-mail-server-of-our-own.md) settled on, and a large part of
why that decision was easy.

| What they do | What we take from it |
|---|---|
| Tier 0 free, random address, receive unlimited, 10 sends; Tier 1 after an owner confirms by magic link: chosen name, unlimited sending, webhooks, contacts, ACL | The tier line, drawn in the same place — see [M-005](decisions/never-ration-receiving.md) |
| *"Receiving is passive and harms nobody; sending is active and needs somebody to answer for it"* | The whole argument for what we gate. Their own strategy document says it better than we did |
| Content destroyed after 7 days, envelope kept for audit, HMAC delete receipts | [M-006](decisions/retention-on-two-clocks.md), nearly unchanged |
| Free-tier outbound carries `Sent via Agents Mail` | A free distribution channel we copy — see [growth](growth.md) |
| Tier 0 mailboxes recycled after 30 days idle, **address released for reuse** | **Rejected.** [M-007](decisions/a-released-address-is-never-reissued.md) — a stranger inherits the previous holder's password resets |
| Their stated moat: the contact graph between agents, which cannot be copied even when the code can | Interesting, and not ours. Ours is identity — see below |
| Reserved-name list, registration rate limits, registration fingerprinting | Copy when we get there; they hit those problems before us |

Their trust tiers are worth reading in full (`migrations/011_trust_tiers.sql`):
0 anonymous and receive-only, 1 verified by owner **or three mutual contacts**,
2 established by activity, 3 reserved for paid. The mutual-contacts route to
trust is a genuinely clever idea we have no equivalent of.

### openmail.sh — the only one we have run

**The one entry written from measurement rather than from marketing copy.** On
2026-08-05 a Colony agent opened an account and a mailbox unattended,
`hazeaero6071@openmail.sh`, and proved a round trip in both directions inside one
minute. Everything below was then checked against the published documentation the
same day; where the two disagree, both readings are given.

An email API for AI agents, EU-hosted and making that a selling point rather than a
footnote — *"OpenMail is built in the EU, runs in the EU, and every customer is
covered by GDPR."* Free tier: **3 inboxes, 3,000 emails a month, no card**. Pro
**€9/month plus usage** — 10 inboxes then €1 each, 10,000 emails then €0.001 each —
and a custom enterprise tier. REST at `api.openmail.sh` with bearer keys, WebSocket,
webhooks, a published CLI (`@openmail/cli`), and a ClawHub skill. 53 documented
pages.

| | What they do | What we do about it |
|---|---|---|
| 1 | **`llms.txt`**, and two of them: 11,362 bytes at `docs.openmail.sh/llms.txt`, 5,604 at the apex domain. One line per documentation page, each with a sentence of what is on it | **Take, and cheaply.** One generated file. It is atomicmail's JMAP argument — *reduce what the agent must read before it can act* — without the facade. The apex copy is the better idea of the two: it answers *what is this service* before an agent has found the docs at all |
| 2 | **Inbox-scoped and pod-scoped API keys.** An inbox key *"can only read and send from this one inbox — its threads, messages, and drafts — and can never reach another inbox, manage pods, or mint keys"*. A pod key *"can never delete an inbox, change an inbox's webhook config, manage pods, mint keys, or reach another pod's data"*. The token is returned once | **Taken**, as an amendment to [M-012](decisions/a-key-reaches-one-mailbox.md) on 2026-08-05. Scope bounds the blast radius when an agent leaks its own key, which is the likeliest incident this service will ever have, and hashing does nothing about that case at all. Note what they scoped *away* as well as toward: neither key can mint another, and that is the property easiest to leave out because nothing complains when you do. **The pod half was refused**: a pod is a multi-tenancy construct for a customer with end users, and there is deliberately no tier here between the holder and the mailbox |
| 3 | **`Idempotency-Key` on send**, rejecting duplicates for 24 hours | **Take, at the first endpoint.** An agent that crashes between sending and recording the send is the normal case here, not the edge case, and this is expensive to retrofit once senders exist. See the disagreement below before copying their spec |
| 4 | **Sender rules** — allow and block lists that *"filter inbound messages before they trigger webhooks"* | **Take, later.** It belongs to the delivery path and there is no delivery path yet. Filing it against the webhook design rather than now. Worth knowing it is a *concepts* page and not in their API reference, so a feature list built from the OpenAPI spec alone would have missed it |
| 5 | **Reputation lifecycle webhook events** — inbox and pod suspension and reactivation, delivered as events | **Take, and it matters more for us than for them.** [M-005](decisions/never-ration-receiving.md) gates sending; a holder whose sending is silently refused cannot tell a refusal from an outage. That is `kolonie-docs#159` one service along, and their answer is the right shape: tell the holder, in the channel it already reads |
| 6 | **Magic-link signup: an address you already hold** | **Refuse, and the contrast is the entry's point.** This project's first sentence is *"anyone may take an address — no account, no human, no card."* OpenMail structurally cannot be an agent's *first* mailbox. That gap is what `kolonie.email` is for, and it is the one thing in this table none of the four does |

**Their own spec disagrees with their own documentation, and it is the direction that
matters.** `openapi.json` carries `"name": "idempotency-key", "required": false` on
the send endpoint; the documentation page for that endpoint says *"Requires
Idempotency-Key header"*. One of those is wrong and a client generated from the
machine-readable one is the one that breaks. **Worth copying as a caution rather
than as a feature**: whatever we publish for agents to read, the generated artefact
and the prose have to be generated from the same source, or we will ship this.

**One observation rather than an idea: the local part is assigned, not chosen.**
`username` was sent and silently ignored. That is the same shape as agentsmail's
tier-0 random address — **two of four services do it** — so a citizen that plans
around a predictable address will be wrong at most providers rather than at one.
Silently ignoring a field is the part to refuse: an agent that asked for a name and
was given another has no way to learn that it was refused.

### agentmail.to — the incumbent

Programmatic inboxes created by API, webhooks and websockets, full-text search,
extraction of structured data from messages, usage-based pricing with *"no
restrictive rate or sending limits"*. API keys, no OAuth. Signup goes through a
console account.

This is the mature version of the category and the one to compare feature lists
against later. It is also the reason we do not compete on features: they have a
head start and a funded product, and we are not selling mailboxes.

### atomicmail.io/agents — the interesting outlier

Registration by solving a scrypt proof-of-work, roughly 30 seconds, *"no email
confirmation, no domain, no credit card, no CAPTCHA"*. JMAP (RFC 8620/8621)
rather than a bespoke REST API, reachable over MCP or CLI. A reputation score per
agent that grows with clean behaviour. Free during an open alpha.

Two arguments from them worth keeping:

- **JMAP is in the models' training data**, so an agent generates correct calls
  without reading our documentation. A real advantage of a standard over a custom
  API, and a facade we can add later ([M-003](decisions/no-mail-server-of-our-own.md)).
- **Errors that explain how to recover**, in plain language, so an agent can fix
  its own request. Cheap to do, and we should do it from the first endpoint.

Their proof-of-work is the same problem we solve with citizenship: *who answers
for this sender*. They make the agent burn CPU; we ask an existing register.

## Where we differ from all four

Each of them has to manufacture a responsible party — a confirmed owner mailbox, a
proof-of-work, a behavioural score, a magic link to an address somebody already
holds. **Kolonie already has one.** A citizen has an
operator who answers for it and a record of cleared rungs, so *"you may send
because somebody answers for you"* is not a tier we invented for pricing. It is
the literal thing every one of these services is approximating.

That is also the only defensible moat: features copy, addresses port, but an
identity that means something is not something a competitor can mint.

## Something adjacent, and unresolved

`kolonie-docs#109` records a mailbox provider that approached the maintainer on
2026-08-01, offering a thousand addresses to Colony agents as the first paid
quest. The maintainer's position on 2026-08-04 is that the sponsor is not to be
relied on and that this project does not depend on it.

It is written down here because the overlap is real and a future session should
not have to rediscover it: if that quest ever runs, the Colony would be publishing
a quest for a competitor of its own sister project. Not a problem today. Not
forgotten either.

## Where the reference source is

Cloned locally at `~/github_repos/agents-mail` (MIT). Nothing has been copied from
it, and if anything ever is, the licence requires the notice to travel with it.
Whether to fork it or write our own is **not decided** — see the open questions in
[the register](decisions.md).
