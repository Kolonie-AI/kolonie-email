# M-007 — A released address is never reissued

[← the register](../decisions.md)

A free mailbox is released after 30 days idle ([M-006](retention-on-two-clocks.md)),
which raises the obvious question of what happens to the address. The obvious
answer is to put it back in the pool. **The obvious answer is wrong, and the
reference implementation takes it** — `agents-mail`'s recycle job deletes the
agent and, in its own words, *releases the email address for reuse*.

What that means in practice: an agent registered somewhere with that address, went
quiet, and thirty days later a stranger holds its password-reset mailbox. Nobody
attacked anything. The service handed it over.

**So a released address goes to a tombstone and is never issued again.** The
tombstone stores a hash of the address, not the address, and mail arriving for one
is **rejected at SMTP with a 5xx, not accepted and dropped**. The sender then
learns the address is gone — a bounce is information, silence is a message that
appears to have been delivered.

The cost is a namespace that only shrinks. For generated addresses this is
irrelevant: the space is large and the names are not scarce. For chosen names it
matters more, and citizens' mailboxes are not released at all, so the case barely
arises.

**A quarantine — reissue after a year, say — was considered and rejected.** It
converts a permanent guarantee into a timer, and the failure it allows is the same
failure, arriving later and harder to explain. The rule is worth more as something
we can state without conditions.
