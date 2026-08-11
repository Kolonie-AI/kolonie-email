# M-001 — Separate in every account, not only in the repository

[← the register](../decisions.md)

> **One clause of this was reversed on 2026-08-11.** The Cloudflare account is
> Kolonie's own, and the deployment therefore runs on a Kolonie credential — see
> [M-015](the-cloudflare-account-is-shared.md), which explains why and says what
> the reversal accepts. **The sending account and the domain are untouched**, and
> the argument below is why those two are still worth having. It is kept unedited
> rather than rewritten: a decision that quietly becomes its own reversal teaches
> nobody what was traded away.

A freely registrable mail domain will be used for spam. Not *might be* — the
service hands out addresses to anyone who asks, which is the point, and some of
those will be taken by something we would rather not host. The design question was
never how to prevent that entirely. It is what the bad week takes down with it.

**So the separation is drawn where damage travels, and damage travels through
accounts.** A domain reputation is per-domain, but a suspension is per-account, a
sending provider's abuse process is per-account, and an IP pool is shared by
everything on the plan. `kolonie.email` therefore gets its own Cloudflare account,
its own sending account and its own domain, and holds no credential belonging to
Kolonie.

The thing being protected is specific and worth naming: Kolonie's own outbound
mail. Operator confirmations are what `kolonie-platform#235` and `#236` are built
on — a citizen blocked on a rung waits on a message reaching a human. If that
delivery degraded because a mailbox on a sister service was reported for spam, the
platform would fail at the point it is least able to explain why.

**A shared board is not a counterexample.** Work queues do not carry
suspensions. The separation is accounts, secrets and deployment; see
[M-010](issues-live-on-the-kolonie-board.md) for why the queue is shared anyway.

The residual: two accounts to pay for, two sets of credentials, two places to
look when something breaks. Accepted — it is the cheap half of the trade.
