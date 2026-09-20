# Repository policy

## Scope

This repository is a maintained fork of `jarrodwatts/claude-hud`. Preserve an
easy-to-review relationship with upstream while carrying only deliberate fork
changes. [`CLAUDE.md`](CLAUDE.md) remains the upstream-shaped build and
architecture guide.

## Boundaries

- Keep the plugin public-safe. Never commit credentials, transcript contents,
  local Claude configuration, provider payloads, or generated user state.
- Prefer upstream-compatible source changes. Separate fork-only behavior and
  document why it cannot be configured or contributed upstream.
- Edit `src/`, tests, documentation, or metadata as owned by the change; do not
  include generated `dist/` changes in ordinary pull requests.
- Keep versions aligned across `package.json` and both plugin manifests when a
  release changes the public version.

## Host statusline wiring

Hosts run this fork by pointing `statusLine.command` in `~/.claude/settings.json`
directly at a local checkout:

```text
"$HOME/Project/sympoies/claude-hud/src/index.ts"   (bun)
```

That path is the only thing that delivers the fork's rendering changes. The
installed `claude-hud` plugin supplies the `setup` and `configure` commands
only; it does not feed the statusline, and its `commands/` are currently
identical to upstream's, so the marketplace a host installs from does not
affect what the HUD renders.

- Do not run `/claude-hud:setup` on a host wired this way. It rewrites
  `statusLine.command` to resolve
  `~/.claude/plugins/cache/*/claude-hud/*/` and take the highest version, which
  silently detaches the statusline from the checkout — the HUD keeps working,
  so the loss of the fork's rendering is easy to miss.
- Recover by restoring the checkout path above; no reinstall is needed.
- Installing this fork as a second marketplace does not protect against that.
  Both copies would land under the same `plugins/cache/*/claude-hud/*/` glob and
  the winner would be decided by version number, so prefer a single marketplace.

## Working agreement

- Read [`CLAUDE.md`](CLAUDE.md) for architecture and build details and
  [`CONTRIBUTING.md`](CONTRIBUTING.md) for the upstream contribution shape.
- Inspect affected parsers, renderers, fixtures, snapshots, and plugin metadata
  before editing. Prefer focused behavior tests.
- Run `npm test` before delivery; it builds the TypeScript output and runs the
  Node test suite.
- Deliver fork changes through the governed managed-worktree and PR flow. Do
  not submit a third-party upstream issue or PR without a separate human
  decision under the upstream-contribution policy.
