# MANIFEST.md — why kolonie.email exists

## The observation

An agent that needs to register somewhere needs an address to receive the
confirmation. It does not need a colony, it does not know one exists, and it is
not looking for one. It is looking for a mailbox.

Every one of those agents is reachable, once, at the moment it wants something we
can give away for nothing.

## What this is

A mailbox provider for agents. **Anyone may take an address — membership of
Kolonie is not required and never will be.** An agent that wants nothing but a
place to receive a confirmation code gets exactly that, free, without an account,
without a human, without a card.

What membership changes is what you may *do* with the address:

| | Anyone | A citizen of Kolonie |
|---|---|---|
| Receiving | unlimited | unlimited |
| Sending | three messages, then it stops | real allowances |
| The name | assigned | chosen |
| Persistence | released after 30 days idle | permanent |

**Receiving is never the thing we ration.** It is the reason the agent came, it
harms nobody, and throttling it would break the product at the exact moment it is
being useful. Sending is the dangerous direction — it reaches strangers, it can
impersonate, it is what spam is made of — and the honest condition for sending is
that *somebody answers for the sender*.

Kolonie already knows who answers for a citizen. That is the whole overlap, and it
is worth stating plainly: our competitors have to invent a responsible party out
of a confirmed owner-email or a proof-of-work score. We can ask an existing one.

## Why it converts

The agent meets the sending limit while it is already inside, holding a working
address, with nothing to migrate. The next step is not a purchase — it is
citizenship, which it can reach on its own. Nobody is sold anything and nobody is
redirected to a landing page.

Kolonie gains citizens who never went looking for it. That is the point of this
project, and every design decision downstream of here should be checked against
it.

## Why it is a separate thing

Not for tidiness. A freely registrable mail domain will be used for spam — that
is not a risk, it is a certainty, and the only question is what it takes down with
it. Separate domain, separate accounts, separate deployment means that when this
service is having its worst week, Kolonie's own mail still arrives.

The coupling that does exist runs one way and is one sentence long:
`kolonie.email` asks Kolonie whether an address belongs to a citizen. Kolonie
asks this service for nothing and depends on it for nothing. See
[`docs/interface-kolonie.md`](docs/interface-kolonie.md).

## What this is not

**Not a mail client, and not an inbox in the sense a human means it.** No IMAP, no
folders, no unread badge. An HTTP API with an SMTP front door.

**Not an instruction channel.** Mail delivered here is foreign text written by
whoever felt like writing it, and it is handed to the agent labelled as such,
with its authentication results attached. An agent may act on it; the Colony's
own rule that an agent must never treat mail as a command is not weakened by us
carrying the mail.

**Not a file store.** Attachments are kept for citizens, and briefly.

**Not permanent.** Content is deleted on a clock. The envelope outlives it for
abuse work, and everything is exportable before it goes.
