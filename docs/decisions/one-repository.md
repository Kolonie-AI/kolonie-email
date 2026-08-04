# M-002 — One repository for the whole service

[← the register](../decisions.md)

Code, schema, infrastructure configuration, website and documentation live in
this one repository. The obvious alternative — a repository each for the service,
the infrastructure and the docs, mirroring how Kolonie is laid out — was rejected.

**Kolonie is split because two of its repositories span the others.**
`kolonie-docs` describes every repository and `kolonie-infra` deploys several of
them; neither belongs inside any one of them. Nothing here spans anything. A
split would buy three issue trackers, three clone steps and a cross-repository
pull request for every feature that touches a schema and an endpoint at once,
which is most of them.

The reference implementation we studied — `huberthe-pro/agents-mail`, MIT — keeps
API, CLI, SDK, admin panel, landing page, migrations and docs in a single
repository at a comparable size, and it is not visibly suffering for it.

**Splitting later is a day's work.** The triggers worth watching: the SDK being
published for outsiders on its own release cadence, or the website acquiring
contributors who should not have commit rights to the mail path. Neither is true
today, and building for either now is building for a guess.

Documentation is the one case worth stating explicitly, because Kolonie's shape
suggests otherwise: docs about governance span repositories, docs about this
service do not. They stay here. What lives on the Kolonie side is a single note
recording why this project is outside — and nothing else.
