# M-010 — Issues live on the existing Kolonie board

[← the register](../decisions.md)

This repository has no project board of its own. Its issues go on project 1 of
the `Kolonie-AI` organisation, labelled **`area:mail`**, with a filtered view for
anyone who wants to see only this service.

The first proposal was a separate board, on the theory that a separate project
should be separate everywhere. That confuses two different kinds of separation.
**What has to stay apart is what can transmit damage — accounts, secrets,
deployment ([M-001](separate-in-every-account.md)). A work queue transmits
nothing.** No suspension, no blocklisting and no leaked credential travels through
a column on a board.

What a second board would cost is concrete: a second place to look, a second
In-Progress ritual, and two queues maintained by the same small set of agents who
already have a routine built around one.

**The taxonomy already fits.** The board carries `area:platform`; `area:mail` is
one more value in a field that exists, not a new mechanism.

**One effect is worth having on purpose:** a `p1` here now competes visibly with a
`p1` on the platform. Splitting them across two boards would not remove the
competition, only the ability to see it — and this project must not quietly eat
the MVP's capacity.

**There will be no auto-add rule, and this is not fixable.** GitHub caps a project
at five auto-add workflows and Kolonie has spent all five
(`kolonie-docs/AGENTS.md`, measured 2026-08-03). This repository therefore arrives
uncovered by construction, like every repository the Colony has added since — an
issue opened here never reaches the board and nothing says so.

So the rule for this repository is the same one `kolonie-docs` already states:
**open the issue and put it on the board in the same breath.**

```bash
gh project item-add 1 --owner Kolonie-AI \
  --url https://github.com/Kolonie-AI/kolonie-email/issues/<n>
```
