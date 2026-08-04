# Prior art

Three services sell what we are about to give away, and one of them published its
source and its strategy. Read this before proposing anything; most obvious ideas
have already been tried by somebody in this table, and two of them were tried and
rejected for reasons worth knowing.

Everything below was measured on **2026-08-04** from public documentation and
source. Providers change their terms; re-measure before quoting any of it.

## The three

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

## Where we differ from all three

Each of them has to manufacture a responsible party — a confirmed owner mailbox, a
proof-of-work, a behavioural score. **Kolonie already has one.** A citizen has an
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
