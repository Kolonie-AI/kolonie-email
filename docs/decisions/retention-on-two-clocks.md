# M-006 — Retention runs on two clocks

[← the register](../decisions.md)

Agents do not need two years of mail. Almost every message here is a confirmation
code that mattered for four minutes. Keeping it forever costs storage, creates a
liability over other people's data, and serves nobody.

**Two clocks, because content and address expire for different reasons.**

| | Anyone | Citizen |
|---|---|---|
| Content — body, attachments | 7 days | 90 days |
| Envelope — from, to, time, size, message-id | 12 months | 12 months |
| The mailbox itself | released after 30 days idle | permanent |

The envelope outliving the content is what makes abuse work possible after the
fact — who sent a thousand messages last Tuesday is answerable without keeping
what they said. `agents-mail` does the same thing under the name *destroy
content, preserve envelope for audit*, and it is the right shape.

**`expires_at` belongs in every API response that returns a message.** An agent
whose mail vanishes silently has hit a bug; an agent that read the expiry has a
rule it can plan around. This is the difference between a retention policy and
data loss, and it costs one field.

**Backups must rotate faster than the shortest retention, or the retention is a
claim rather than a fact.** A backup holding deleted content for 90 days makes
the 7-day promise false, and nobody would find out until it mattered. Whatever
backup scheme we adopt is constrained by this row, not the other way round.

Deletion issues a receipt — an HMAC over the message id and the time it was
destroyed. Cheap to produce, verifiable by a machine, and the only form of "we
deleted it" that is worth more than a sentence on a website.

Two consequences that are not obvious:

- **Retention is what keeps the service inside Cloudflare's included 5 GB**
  ([M-003](no-mail-server-of-our-own.md)). It is a cost decision as much as a
  privacy one.
- It reduces what we hold about people who never agreed to anything. Inbound mail
  is written by third parties who have no relationship with us; the shortest
  retention that keeps the product working is also the smallest liability.

Everything is exportable in one call before it expires (M-013). A short retention
is only defensible if leaving with your data is easy.
