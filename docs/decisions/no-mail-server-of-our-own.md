# M-003 — No mail server of our own

[← the register](../decisions.md)

The first sketch had a VPS running Postfix as the MX, handing messages over LMTP
to an ingest service, with Stalwart as the alternative for JMAP. Both were
dropped.

**The insight that killed them: we are not building a mail server. We are
building an HTTP API with an SMTP front door.** No agent speaks IMAP. Nothing
here needs maildirs, folders, or a delivery agent. Once that is admitted, most of
a mail server is machinery we would operate and never use.

So the shape is Cloudflare's, and it is the shape the reference implementation
already proved at this scale:

| Stage | What runs |
|-------|-----------|
| Inbound | Email Routing, **catch-all** on the domain, into an Email Worker |
| Parse and store | Worker parses MIME, writes rows to D1, attachments to R2 |
| Read | API Worker over HTTPS — the only interface an agent ever sees |
| Housekeeping | Cron Worker: retention, recycling, tombstones |
| Outbound | See [M-004](outbound-through-a-provider.md) |

**Catch-all is not a convenience, it is forced.** Email Routing allows 200
routing rules per domain (measured 2026-08-04). One rule per mailbox would cap
the service at 200 mailboxes; a single catch-all into a Worker has no such
ceiling and puts address resolution in our database where it belongs.

**What it costs, measured 2026-08-04:** Email Routing free, 25 MiB per inbound
message. Workers Paid at 5 $/month covers 10 M requests and 30 M CPU-ms. D1
includes 5 GB and 25 bn row reads. R2 gives 10 GB before 0.015 $/GB-month. The
service therefore costs about **5 $/month plus the domain** until it is genuinely
busy, and the retention rules in [M-006](retention-on-two-clocks.md) are what keep
storage inside the included 5 GB rather than growing forever.

The maintainer's reason for preferring Cloudflare was that it has already proved
itself in this project. That agrees with the technical argument, so this was not a
close call.

**JMAP is a facade, not a foundation.** Atomic Mail's argument for it — that the
protocol is in the models' training data, so agents generate correct calls without
reading docs — is a good one and may be worth answering later. It can be added in
front of this without changing any of it.
