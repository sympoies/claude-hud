# Remaining-capacity status

Design and sources checked on 2026-10-08. The shared display contract is:
model, context, 5-hour allowance left, weekly allowance left, and session identity
or branch when available. Quotas report **remaining**, never consumed, capacity.
An unavailable quota is omitted; it must not become a fictitious 100% left.

## Claude Code

Keep Claude HUD as the statusLine renderer. The [standard display configuration](host-config.json)
already selects `display.usageValue = "remaining"` and `sevenDayThreshold = 0`.
This shows both populated windows throughout their lifecycle. The bar drains as
capacity is consumed, the number says `% left`, and an exhausted window retains
`0% left` plus its warning and reset time. The other window stays visible.
Explicit `percent` mode remains available for users who prefer consumed usage.

Claude supplies `rate_limits.five_hour` and `rate_limits.seven_day` through
statusLine JSON. See the [official statusline data reference](https://code.claude.com/docs/en/statusline#available-data).
Context remains a used percentage in this preset: its `Context N%` text is a
consumer contract independent of account quota. Do not change it when applying
a remaining-quota preset. `showSessionName` can add native session identity.

## Codex CLI

Use the [native configuration fragment](codex-config.toml). Merge its `status_line`
key into the existing `[tui]` table in the active `config.toml`; do not replace
other configuration or append a duplicate table. `/statusline` offers the same
selection interactively. No Claude plugin, external usage scraper, or hook is
needed. Native `five-hour-limit` and `weekly-limit` fields show remaining
primary and secondary allowance. They disappear when the account supplies no
such window; window duration and availability are provider-controlled.

Codex context uses `context-remaining`, yielding `Context N% left`. Native
`session-id` and `git-branch` supply identity/work context. These choices preserve
consumers that recognize `Context N%` as used and `Context N% left` as remaining.
The preset is a CLI footer configuration; it does not install a custom Desktop
or IDE pane.

Sources: [OpenAI configuration reference](https://developers.openai.com/codex/config-reference#tui)
and [native status-line fields at v0.160.1](https://github.com/openai/codex/blob/rust-v0.160.1/codex-rs/tui/src/bottom_pane/status_line_setup.rs).
[OpenAI's changelog](https://learn.chatgpt.com/docs/changelog) records CLI releases;
this preset was checked against [CLI v0.160.1](https://github.com/openai/codex/releases/tag/rust-v0.160.1)
(published 2026-10-05), not inferred from Desktop features.

## Does a Claude mod replace the HUD?

**Facts:** a mod is an in-process JavaScript/TypeScript plugin using function
hooks. It can draw a pane, a band above the prompt, and other interface sites;
`session.measure` observes context and quota changes. Mods run in Claude Code
CLI and Desktop's Code tab. CLI support is enabled by default from v2.1.287;
Desktop from v2.1.286. See [Anthropic's overview](https://code.claude.com/docs/en/plugins/mods/overview),
[API reference](https://code.claude.com/docs/en/plugins/mods/reference), and
[v2.1.287 release](https://github.com/anthropics/claude-code/releases/tag/v2.1.287)
(published 2026-10-01).

**Inference and decision:** a custom mod could implement these indicators, but
that capability does not establish that an existing mod preserves every HUD
feature or the shell-output contract. A new renderer is unnecessary for quota
parity. Retain the current renderer and use Codex's native counterpart. There is
no official source here establishing a ready-made mod as a complete replacement.

Codex also has [lifecycle hooks](https://learn.chatgpt.com/docs/hooks), but its
documented footer selects native identifiers rather than running a shell
renderer. Managed session roles, unread mailbox counts, and issue references
have no documented arbitrary Codex footer field. An external session board or
mailbox command is the closest shared equivalent. This preset adds no
Claude-only panel for that optional metadata.

## Acceptance and rollout

Tests cover both layouts, either exhausted quota, missing windows, the standard
Claude preset, and the compact context contract. After applying configuration,
check both harnesses in a live terminal: fresh quota, partially consumed quota,
and a missing window. Confirm the quota numbers say left and context consumers
still parse correctly. Narrow terminals may truncate native footer items;
put quotas before optional identity fields. Unit validation is not live UI
acceptance. Configuration rollout is separate from merging this source change.
