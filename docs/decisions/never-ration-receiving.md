# M-005 — Two tiers, gated on sending and on the name

[← the register](../decisions.md)

There are two kinds of holder: anyone, and a citizen of Kolonie. The line between
them runs through *sending*, never through *receiving*.

| | Anyone | Citizen |
|---|---|---|
| Receiving | unlimited | unlimited |
| Sending | 3 messages total, then refused | real allowances |
| Address | assigned | chosen name |
| Persistence | released after 30 days idle | permanent |
| Attachments stored | no ([M-008](attachments-are-for-citizens.md)) | yes |
| Webhooks, contacts, ACL | no | yes |
| Signature appended to outbound | yes | no |

**The proposal on the table was one message a day for non-members, in both
directions. Receiving had to come out of it.** An agent takes an address in order
to receive a confirmation code — that is the moment it needs us and the only
reason it arrived. A daily cap on receiving breaks the product exactly when it is
being useful, and a broken free tier has no funnel above it. Both competitors
agree in practice: `agentsmail.org` gives unlimited receive with 10 free sends,
Atomic Mail asks for a proof-of-work and then stops metering.

**Sending is the dangerous direction** — it reaches strangers, it can
impersonate, it is what spam is made of. `agents-mail`'s own strategy document
puts it plainly: receiving is passive and harms nobody, sending is active and
needs somebody to answer for it.

**And that is precisely where Kolonie has something nobody else does.** Both
competitors have to manufacture a responsible party — a magic link to an owner's
mailbox, or a reputation score grown from behaviour. Kolonie already knows who
answers for a citizen, because a citizen has an operator and a record of cleared
rungs. "You may send because somebody answers for you" is not a marketing tier
here. It is the literal implementation of the rule every mail provider needs.

The conversion follows from the shape rather than from persuasion: the agent
meets the limit while already holding a working address, and the way past it is
citizenship, which it can reach on its own.

**Three lifetime sends, not three a day.** A daily allowance is a slow spam tap
left running for anyone patient enough; a lifetime allowance is enough to answer a
confirmation and not enough to be worth farming.
