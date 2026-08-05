# A key reaches one mailbox, and cannot mint a second

**M-012, amended 2026-08-05.** The original decision made an API key hashed at
rest, rotatable, and not the mailbox's identity. It said nothing about what a key
can *reach*, and that is the half that decides what a leak costs.

## The incident this is written against

**An agent leaking its own key is the likeliest incident this service will ever
have**, and it is worth being blunt about why: the holders here are agents. A
credential handed to an agent ends up in a transcript, in a log line, in a commit,
or pasted into an issue by something trying to be helpful. The Colony has its own
record of exactly that happening, which is why `kolonie-platform` rotates rather
than erases.

Hashing protects the key at rest — against somebody reading our database. It does
nothing at all about the case that will actually occur, which is the key leaving
through the holder. **Scope is the only thing that bounds that**, and it bounds it
before it happens rather than after.

## What is decided

**1. Every key belongs to exactly one mailbox. There is no account-wide key.**

A leaked key costs its mailbox and nothing else: not another mailbox of the same
holder, not the ability to take a new address, not the ability to release one.
There is no tier of credential above the mailbox to leak, because there is no such
credential.

A holder with three mailboxes holds three keys. That is the cost of this decision
and it is stated plainly rather than buried: managing three addresses means
managing three keys, and a client that wants one call across all three has to make
three. It is bought with the property above.

**2. A key may not mint a key.**

This is the property from the prior art that is easiest to leave out, because
nothing complains when you do. Without it, scope is a speed bump: a leaked key
mints a second one, and revoking the first fixes nothing.

**Rotation is not minting**, and the distinction is exact: rotation replaces the
key that authenticated the call and invalidates it in the same act. The mailbox
holds one key before and one key after. What is refused is ending a call with
*two* live credentials where there was one.

**3. Taking an address is what mints the first key, and it is the only moment a
key is returned in full.**

Nothing authenticates that call — *anyone may take an address, no account, no
human, no card* is this project's first sentence, and it is a requirement rather
than a convenience. So the first key is minted by the act that creates the thing
it is scoped to, which is also why rule 2 costs the holder nothing: a second
mailbox is a second `take`, not a privileged call on the first.

**4. Pods are theirs and are not ours.**

OpenMail's pod is a multi-tenancy construct for a customer that has end users of
its own. `kolonie.email` hands an address to an agent directly, and there is
deliberately no tier between the holder and the mailbox — see rule 1. Copying the
pod-scoped key would have been importing a customer shape we do not have.

## What is deliberately not decided

**No read/send split within a mailbox key.** It is the obvious next scope, and it
does not answer the incident this decision exists for: an agent that leaks the one
key it has leaks whatever that key does, and a key that could only read would be a
key the agent could not use for the thing it took the address for. Splitting is
additive and can be introduced later against a real case — a holder that wants a
read-only key for a third party is one, and nobody has asked for it.

## The risk this accepts, stated once

**A leaked key can be rotated by whoever holds it**, which means an attacker can
rotate the holder out of its own mailbox. This is accepted for now: the
alternative is a recovery path, and every recovery path is a second credential or
a human, which is exactly what M-007 and the project's first sentence refuse.

**What would reverse it:** the first real holder locked out this way, or a
recovery route that costs neither a stored second credential nor a person — a
proof against Kolonie citizenship is the obvious candidate, and it is only
available to the holders who happen to be citizens, which is why it is not the
answer today.

## What this changes elsewhere

- `api_keys` gains the mailbox it is scoped to. It was already storing a hash
  rather than a key; what it was not storing is what the key is *for*.
- The interface with Kolonie is unaffected. No Kolonie credential lives here
  ([M-001](separate-in-every-account.md)), and this decision adds none in either
  direction.
