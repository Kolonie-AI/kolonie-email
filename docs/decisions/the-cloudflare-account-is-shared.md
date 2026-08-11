# M-015 — The Cloudflare account is Kolonie's, and the blast radius is accepted

[← the register](../decisions.md)

**Decided by the maintainer on 2026-08-11.** It reverses one clause of
[M-001](separate-in-every-account.md) and nothing else: `kolonie.email` runs in
**Kolonie's existing Cloudflare account**. The domain, the sending account and the
absence of any other shared credential are untouched.

## Why the clause was reversed rather than argued down

M-001 is not wrong about how damage travels. It is right, and the reversal
concedes the point: a suspension is per account, an abuse process is per account,
and a bad week in this account is a bad week for `kolonie.ai` too.

What changed is the price of being right. A second Cloudflare account is a
maintainer action — a signup, a payment method, a token minted and delivered — and
[#1](https://github.com/Kolonie-AI/kolonie-email/issues/1) sat on it from
2026-08-04 to 2026-08-11 while every deployable piece of this project waited
behind it. The two development agents already hold full access to the existing
account. So the choice was not *isolated* against *shared*; it was *shared now*
against *isolated at some later date, with nothing built in between*.

**Measured 2026-08-11:** `kolonie.email` is already a zone in that account, on the
Free plan, alongside `kolonie.ai` and `kolonie.sh`. The isolation M-001 asked for
had never existed; what the block bought was not separation but delay.

## What is accepted, said plainly

An account-wide suspension, a billing failure or an agent's mistake in this
account reaches **the Colony's own zone and everything else in it**, not only this
service. That is the exposure M-001 was written to prevent, and it is now accepted
knowingly and with a date on it.

It is reversible in one direction: moving a zone into a new account later is work,
but it is ordinary work, and every decision below the account line — the schema,
the ingest path, the retention clocks — survives the move untouched.

## What still stands

- **The sending account stays separate** ([M-004](outbound-through-a-provider.md)).
  It is the one that carries the reputation an abuse report destroys, and nothing
  in this reversal touches it. Reversing that one is a separate maintainer
  decision.
- **The domain stays this project's own** ([M-011](the-domain-is-kolonie-email.md)).
- **No secret enters this repository** (`AGENTS.md` §5). A shared account makes the
  token that reaches it more valuable, not less.
- **Kolonie may not be made to depend on this service.** A shared account is not a
  shared runtime; the interface still runs one way
  ([`interface-kolonie.md`](../interface-kolonie.md)).

## What it unblocks

Everything that touches deployment: the zone's Email Routing, the Worker, D1, R2.
[#1](https://github.com/Kolonie-AI/kolonie-email/issues/1) is closed and the
receiving path can be built.
