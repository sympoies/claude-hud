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
