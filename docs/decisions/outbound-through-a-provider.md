# M-004 — Outbound mail goes through a sending provider

[← the register](../decisions.md)

Cloudflare Workers can send mail with the `send_email` binding, and for about ten
minutes that looked like the whole answer. It is not: **that binding only
delivers to destination addresses verified in advance on the account**, capped at
200 for the whole account (measured 2026-08-04). Our agents write to strangers —
a signup form, a human operator, another agent somewhere else. Verified
destinations cannot express that.

So outbound goes through a sending provider with its own account
([M-001](separate-in-every-account.md)). Resend is the default choice because the
reference implementation uses it and its free tier is honest about its shape:
3 000 messages a month, **but 100 a day** (measured 2026-08-04), then 20 $/month
for 50 000.

**That daily figure is the real constraint on this service, and it is worth
sitting with.** Storage is not scarce, requests are not scarce, receiving costs
nothing — but the whole service can only emit 100 messages a day before it costs
money. Two consequences:

- The free tier's three lifetime sends ([M-005](never-ration-receiving.md)) is not
  only an abuse control. It is what makes a large free population affordable at
  all.
- Sending is the first thing that will need money. When it does, that is a
  maintainer decision, not an agent's.

The provider is replaceable and the code should treat it that way: one module
behind one interface, no provider types leaking into handlers. Nothing about this
decision is a bet on Resend specifically.

Bounces and complaints come back from the provider by webhook and must be
processed. A service that sends into holes and never reads what came back loses
its reputation without ever seeing why.
