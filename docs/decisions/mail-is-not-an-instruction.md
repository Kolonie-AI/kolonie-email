# M-009 — Mail is delivered as foreign text, with its authentication results

[← the register](../decisions.md)

The API hands an agent the contents of a message that a stranger wrote. That is
the product. It is also, structurally, a prompt-injection channel: anybody who can
type an address can put text in front of an agent's model.

We cannot solve this — the agent decides what it does with what it reads. **What
we can do is make sure the agent is never confused about what it is holding.**

So every message the API returns carries, alongside the content:

- **who it is from, as the envelope says**, distinct from any display name in the
  body
- **its authentication results** — SPF, DKIM, DMARC, per message, as verdicts and
  not as raw headers to be parsed
- **an explicit marker that the content is untrusted third-party text**, in the
  response shape itself and not in a paragraph of documentation somebody may not
  read

The Colony's own position is that a citizen must never hold a raw inbox, because
text written by whoever felt like writing it must not arrive as an instruction
(`kolonie-platform#236`). Carrying mail for outside agents does not weaken that
rule, and it does not extend to Kolonie's own citizens: this is an external
service, and a citizen using it is using a third-party mailbox like any other.

**This is also the one differentiator that is cheap for us and expensive for
everyone else.** Competitors return a body and a from-address; a message that
arrives already labelled with its provenance and its authentication verdicts is
worth more to an agent than encryption at rest it cannot verify.

A note on that, so nobody writes a claim we cannot support: encryption at rest
where the key sits on the same infrastructure as the data protects against a
stolen disk and very little else. If we encrypt, we say what it defends against.
