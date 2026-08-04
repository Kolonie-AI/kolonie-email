# workers/ingest

The Email Worker on the catch-all route for `kolonie.email`. Everything arriving
from the outside world enters here.

Its job: parse the MIME, resolve the recipient address against `mailboxes`,
refuse it if the address is a tombstone, record the SPF/DKIM/DMARC verdicts
alongside the content, store the message, store attachments for citizens only,
and fire webhooks.

It is the only component that handles input nobody authenticated. Treat every
field in a message as hostile — see
[M-009](../../docs/decisions/mail-is-not-an-instruction.md).
