# Development log

A time-ordered narrative of notable fork-owned work on claude-hud: what
changed, why the fork carries it, the evidence, and links worth keeping for
future maintenance. It complements, rather than duplicates, the repository's
other records:

- Commit messages say what changed. The devlog preserves the non-obvious fork
  context, validation results, and external references that a diff cannot.
- `README.md`, `CLAUDE.md`, and `AGENTS.md` describe the current contract. The
  devlog is an append-only historical narrative; update the canonical current
  document first when behavior or guidance changes.
- Upstream history remains upstream's record. This log covers deliberate fork
  changes and maintenance decisions only.

## When to add an entry

Add one after non-trivial fork work produces a durable outcome worth future
lookup: a fork-only behavior, upstream-sync decision, validated release
milestone, security decision, or incident-relevant finding. Skip upstream-only
changes, generated builds, and same-turn fixes with no future decision value.

## Conventions

- One file per month: `docs/devlog/YYYY-MM.md`, with the newest entry on top.
- Write in English, like the rest of the repository.
- Keep current docs current. The devlog records history; it does not own the
  current plugin contract, fork policy, release process, or upstream state.
- This is a public repository. Never record secrets, transcript contents,
  private configuration, personal identifiers, machine-local paths, provider
  payloads, or credentials. Use public references and neutral descriptions.
- Search past entries with `devlog search <term> [--month YYYY-MM]`. The
  `devlog` binary ships with `nils-cli`; without it, search the month files
  directly.
- When an entry is committed separately, use
  `docs(devlog): <YYYY-MM> - <subject>`.

### Entry template

```md
## YYYY-MM-DD - <short title>

### Result

- What shipped or changed.

### Why / context

- The non-obvious fork or compatibility context.

### Evidence

- Commands run and concrete observations.

### Links

- Commits, issues, pull requests, upstream references, and relevant docs.

### Follow-ups

- Optional.
```

`Result`, `Why / context`, and `Evidence` are required. `Links` and
`Follow-ups` are optional: omit the whole section rather than leaving a
placeholder in it.

## Months

- [2026-09](2026-09.md)
- [2026-07](2026-07.md)
