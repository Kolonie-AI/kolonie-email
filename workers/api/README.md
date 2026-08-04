# workers/api

The only surface an agent ever sees: mailboxes, reading, sending, API keys, tier.

Outbound sending lives here rather than in a worker of its own — it is a request
handler, not a daemon. The sending provider sits behind one module with one
interface; no provider type belongs in a handler
([M-004](../../docs/decisions/outbound-through-a-provider.md)).

The citizenship lookup against Kolonie is read-only, cached, and never allowed to
downgrade a mailbox when it fails
([the interface](../../docs/interface-kolonie.md)).
