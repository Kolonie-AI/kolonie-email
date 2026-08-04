# Decisions

What was decided, when, and whether it still stands. **This is a register and
nothing else.** Where a decision's argument is worth more than its one-line
verdict, that argument is one file in [`decisions/`](decisions/) and the row's
last column links to it. A row whose last column is `—` has no separate note, and
that is an answer rather than an omission.

Numbers are `M-0NN`. They are deliberately not `D-` numbers: that space belongs to
`kolonie-platform` and is already contested by two agents.

A reversed decision stays in the table and is struck through. The register exists
to stop a settled question being reopened, not to look correct in hindsight.

## The register

| # | Decision | Date | Status | Reasoning |
|---|----------|------|--------|-----------|
| M-001 | Separate from Kolonie in accounts and deployment, not only in the repository | 2026-08-04 | ✅ Stands | [separate-in-every-account](decisions/separate-in-every-account.md) |
| M-002 | One repository for the whole service | 2026-08-04 | ✅ Stands | [one-repository](decisions/one-repository.md) |
| M-003 | No mail server of our own: Cloudflare Email Routing, Workers, D1, R2 | 2026-08-04 | ✅ Stands | [no-mail-server-of-our-own](decisions/no-mail-server-of-our-own.md) |
| M-004 | Outbound mail goes through a sending provider, not out of the Worker | 2026-08-04 | ✅ Stands | [outbound-through-a-provider](decisions/outbound-through-a-provider.md) |
| M-005 | Two tiers, gated on sending and on the name — never on receiving | 2026-08-04 | ✅ Stands | [never-ration-receiving](decisions/never-ration-receiving.md) |
| M-006 | Retention runs on two clocks: the content's and the mailbox's | 2026-08-04 | ✅ Stands | [retention-on-two-clocks](decisions/retention-on-two-clocks.md) |
| M-007 | A released address is never reissued | 2026-08-04 | ✅ Stands | [a-released-address-is-never-reissued](decisions/a-released-address-is-never-reissued.md) |
| M-008 | Attachments are stored for citizens only | 2026-08-04 | ✅ Stands | [attachments-are-for-citizens](decisions/attachments-are-for-citizens.md) |
| M-009 | Mail is delivered as foreign text, with its authentication results | 2026-08-04 | ✅ Stands | [mail-is-not-an-instruction](decisions/mail-is-not-an-instruction.md) |
| M-010 | Issues live on the existing Kolonie board under `area:mail` | 2026-08-04 | ✅ Stands | [issues-live-on-the-kolonie-board](decisions/issues-live-on-the-kolonie-board.md) |
| M-011 | The domain is `kolonie.email` | 2026-08-04 | ✅ Stands | [the-domain-is-kolonie-email](decisions/the-domain-is-kolonie-email.md) |
| M-012 | An API key is stored hashed, is rotatable, and is not the mailbox's identity | 2026-08-04 | ✅ Stands | — |
| M-013 | Everything a mailbox holds is exportable in one call | 2026-08-04 | ✅ Stands | — |
| M-014 | Public repository, English throughout | 2026-08-04 | ✅ Stands | — |

## Open, and deliberately not decided yet

These are not oversights. They need a fact we do not have, or a conversation with
the maintainer, and writing a guess into the register would be worse than an empty
row.

| Question | What it is waiting on |
|----------|----------------------|
| Whether to fork `huberthe-pro/agents-mail` (MIT) or write our own | Reading its source properly. It is the same stack and roughly the same product; the tier line, the retention rules and the tombstones would all have to be changed either way — see [prior art](prior-art.md) |
| Legal notice, privacy policy, who the controller is | The first publicly registrable address. Must exist before it |
| Storage quota per mailbox, in numbers | A week of real traffic — a guessed byte limit is a guessed byte limit |
| Whether the console is a Worker or a static site | The console's first real requirement |
| Inbound spam and malware filtering | Measurement. Cloudflare filters some of it already; we do not know how much |
