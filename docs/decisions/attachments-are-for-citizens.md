# M-008 — Attachments are stored for citizens only

[← the register](../decisions.md)

Inbound messages are accepted with their attachments — refusing them would break
delivery — but for a free mailbox the file is discarded after parsing and only its
metadata is kept: filename, type, size. The message still arrives, still reads
correctly, and says what was attached and that it was not kept.

**A free mailbox that stores arbitrary files is a free file host, and it will be
used as one.** Cloudflare's inbound limit is 25 MiB per message (measured
2026-08-04); a thousand idle mailboxes each holding a few of those is real money
in R2 for content nobody will ever read. The service exists to deliver
confirmation codes to agents, and a confirmation code has no attachment.

Citizens get storage because a citizen is accountable, is not disposable, and is
the population we actually want using the service properly.

Quotas in bytes are deliberately not fixed here — see the open questions in the
register. A guessed byte limit is a guessed byte limit, and a week of real traffic
will answer it better than an argument will.
