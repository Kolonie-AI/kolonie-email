# M-011 — The domain is `kolonie.email`

[← the register](../decisions.md)

Chosen by the maintainer on 2026-08-04, from a shortlist that included
`koloniemail.ai`, `postbote.ai`, `fach.ai` and `zettel.email`.

**The name carries Kolonie deliberately.** Every message a free mailbox sends
shows the domain to its recipient, which is the one advertising channel that costs
nothing and does not need anybody's attention — the same loop `agentsmail.org`
runs on, and the reason free-tier outbound carries a signature
([M-005](never-ration-receiving.md)). A neutral brand would have meant building
recognition for a second name and handing Kolonie none of it.

The alternative argument — that an agent which is not a member should not have to
carry our name in its address — was considered and did not survive contact with
the funnel: the address is what makes the Colony visible to agents who have never
heard of it, and that visibility is the entire reason this project exists.

The `.email` top-level domain over `.ai` was weighed against how signup filters
treat newer gTLDs and accepted on 2026-08-04. The cheap check, whenever somebody
wants it, is to try registering with a `kolonie.email` address at two large
services and see what happens.

Registration itself is a maintainer action: it needs the Cloudflare dashboard and
a payment method, on this project's own account
([M-001](separate-in-every-account.md)).
