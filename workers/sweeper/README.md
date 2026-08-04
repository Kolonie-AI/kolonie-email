# workers/sweeper

Cron work, and nothing that answers a request.

- destroy content past its retention, keep the envelope, issue the delete receipt
- release mailboxes idle for 30 days and write their tombstones
- retry citizenship lookups that went stale

Retention is a dedicated component because it runs forever and quietly, and
because getting it wrong is invisible until somebody looks for a message that
should still be there — [M-006](../../docs/decisions/retention-on-two-clocks.md).
