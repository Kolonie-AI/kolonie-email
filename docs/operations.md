# What running this obliges us to

Nothing runs yet, so this is not a runbook. It is the list of things a mail
service must do that a normal web service does not — collected here while we still
remember them, because each one is invisible until it fails.

## Two mailboxes a human must actually answer

`abuse@kolonie.email` and `postmaster@kolonie.email` are required by convention
and expected by every large provider. **An unanswered abuse address is one of the
fastest routes onto a blocklist**, because the complainant's next step is the
blocklist rather than us.

This is the one part of the service that cannot be automated away: a report needs
somebody who can suspend a mailbox and reply. It is a maintainer duty until there
is a rota.

## Bounces and complaints must be read

The sending provider reports back what happened — hard bounces, spam complaints,
feedback loops. A service that sends and never reads the replies loses its
delivery reputation without ever seeing the cause. These become rows in `events`
and they must have consequences: repeated hard bounces stop a mailbox sending.

## Inbound is hostile by definition

We accept mail from anybody, for anybody. Size caps, attachment handling
([M-008](decisions/attachments-are-for-citizens.md)), malware and spam filtering.
Cloudflare filters some of it before we ever see it — how much is unmeasured, and
that measurement is one of the open questions in [the register](decisions.md).

The second hostility is subtler and is ours to handle in the API, not the filter:
mail is untrusted text aimed at a model
([M-009](decisions/mail-is-not-an-instruction.md)).

## Monitoring that is specific to mail

Ordinary uptime checks say nothing about whether mail is arriving. What has to be
watched:

- **our own domain against the public blocklists**, checked on a schedule — the
  first sign of trouble is usually a listing nobody told us about
- delivery and bounce rates from the sending provider
- inbound volume per mailbox, which is how abuse looks before anybody reports it
- the sweeper actually running, because retention failing silently looks exactly
  like retention working ([M-006](decisions/retention-on-two-clocks.md))

## DNS is part of the product

MX, SPF, DKIM, DMARC. A DKIM key that needs rotating, a DMARC policy that starts
permissive and tightens, and MTA-STS if we get that far. **A DNS change here can
stop all mail arriving**, which is why AGENTS.md §7 puts it behind the maintainer.

## Backups are constrained by retention, not the other way round

A backup that holds deleted content longer than the retention window makes the
retention promise false. Whatever scheme we adopt is bounded by
[M-006](decisions/retention-on-two-clocks.md).

## Before the first public address exists

Legal notice, privacy policy, and a named controller for other people's mail. This
is in the register's open questions and it is not optional — it is the one item
that has to be finished *before* launch rather than shortly after.
