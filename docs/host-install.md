# Installing this fork on a host

The maintainer's hosts run this fork directly from a source checkout, not
from the published `claude-hud` plugin package. The plugin only provides the
`setup` and `configure` commands; the statusline itself renders from the
checkout (see "Host statusline wiring" in [AGENTS.md](../AGENTS.md)). A host
needs all four steps below. Skipping step 4 still renders a HUD, but it falls
back to the default display: used instead of remaining usage, and no
cache, token or speed lines. That failure is easy to miss.

## 1. Install bun

- macOS: `brew install bun` (homebrew-core).
- Linux: the distribution's bun or the Homebrew-on-Linux formula, so that
  the binary has a stable absolute path such as
  `/home/linuxbrew/.linuxbrew/bin/bun`.

## 2. Check out the fork

```sh
git clone https://github.com/sympoies/claude-hud.git "$HOME/Project/sympoies/claude-hud"
cd "$HOME/Project/sympoies/claude-hud"
bun install --frozen-lockfile
```

Keep the checkout on `main`; updating the HUD is just a fast-forward of it.

## 3. Point `statusLine` at the checkout

In `~/.claude/settings.json`, and in every per-account settings file if
`CLAUDE_CONFIG_DIR` is used and those files are not symlinks to it, set:

```json
{
  "statusLine": {
    "type": "command",
    "command": "bash -c 'cols=$(stty size </dev/tty 2>/dev/null | awk '\"'\"'{print $2}'\"'\"'); export COLUMNS=$(( ${cols:-120} > 4 ? ${cols:-120} - 4 : 1 )); exec \"<BUN>\" --env-file /dev/null \"$HOME/Project/sympoies/claude-hud/src/index.ts\"'"
  }
}
```

Replace `<BUN>` with the absolute bun path from step 1, for example
`/opt/homebrew/bin/bun`. The `stty` wrapper passes the real terminal width,
which Claude Code does not export to statusline commands.

Do not run `/claude-hud:setup` afterwards; it rewrites this command to the
plugin cache and detaches the checkout.

## 4. Install the display configuration

```sh
mkdir -p ~/.claude/plugins/claude-hud
cp docs/host-config.json ~/.claude/plugins/claude-hud/config.json
```

[`host-config.json`](host-config.json) is the maintainer's standard
configuration: two path levels, git ahead/behind warnings, every optional
line on, and `"usageValue": "remaining"` (usage shown as what is left, in
the battery style). Hosts must use the same file; change it here first,
then copy it to each host.

## Verify

```sh
echo '{"model":{"display_name":"Opus"},"workspace":{"current_dir":"'"$PWD"'"}}' \
  | bun --env-file /dev/null src/index.ts
```

This prints the model and a context bar. In a live session, the usage bar
should read as remaining (close to 100% right after a reset).
