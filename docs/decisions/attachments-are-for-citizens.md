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

## The quotas this comes with

Agreed with the maintainer on 2026-08-04, as **starting values rather than
findings**:

| | Anyone | Citizen |
|---|---|---|
| Stored bytes | 50 MB | 1 GB |
| Messages retained | 200 | no count limit, only the retention clock |
| Attachments | metadata only | stored |

Nothing measured produced these. They are chosen to be generous for a mailbox
that receives confirmation codes and mean for one being used as a drive, and a
week of real traffic should revise them — that revision is a row in the register's
open questions, not a new decision.

The message limit and the retention clock ([M-006](retention-on-two-clocks.md))
both bound the same thing from different directions. That is deliberate: an idle
mailbox is bounded by time, a flooded one by count.
