# handover

A portable **Agent Skill** that makes any AI coding assistant produce a clean,
accurate, self-contained project handover: a continuation prompt the next
session (human or agent) can pick up cold.

It follows the open [Agent Skills](https://agentskills.io) `SKILL.md`
format, so it works across Claude Code, Claude (web/mobile), Cursor, OpenAI
Codex, Gemini CLI, GitHub Copilot, and other tools that support the standard.

## Why use it

- **Cuts token spend on every new session.** Instead of an agent re-reading
  your whole repo, git log, and prior chat to figure out where things stand,
  it starts from a tight, purpose-built summary. Less re-derivation, fewer
  wasted tokens, faster time to useful work.
- **Full context for the next agent, not a vague recap.** The output
  captures what's done, what's in flight, what's next, and what's actually
  been validated, so a fresh agent (or a human) can pick up cold with zero
  guesswork.
- **No lost state between sessions.** Long-running projects survive context
  limits, compaction, and handoffs between different tools or teammates
  without anyone re-explaining the plan.
- **Honest by construction.** It never claims a test passed or a fix
  worked unless that was actually verified in the conversation, so the next
  agent isn't working from a false starting point.
- **Works everywhere, zero lock-in.** One `SKILL.md`, same behavior across
  every Agent-Skills-compatible tool, and it degrades gracefully to a plain
  prompt if a tool doesn't support skills at all.

## What it does

When invoked, the skill:

1. Reads project instructions (`AGENTS.md`, `CLAUDE.md`, etc.) and any
   project-specific handover template already in the conversation, and
   follows that template exactly if one exists.
2. Otherwise falls back to a standard structure: Context, State, Next Steps,
   Validation, Continuation Prompt.
3. States the current outcome first, keeps completed work separate from
   proposed work, and never claims validation that wasn't actually run.
4. Produces a continuation prompt that is immediately actionable on its own.

See [`plugins/handover/skills/handover/SKILL.md`](plugins/handover/skills/handover/SKILL.md)
for the exact instructions the agent follows.

## Install

### Claude Code (plugin marketplace, recommended)

This repo is also a Claude Code plugin marketplace, so you can add it and
install the plugin directly:

```bash
/plugin marketplace add devzl/handover
/plugin install handover@handover
```

Then invoke with `/handover` (namespaced as
`/handover:handover` if you have another skill named `handover`).

To make this available to your whole team, add it to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "handover": {
      "source": { "source": "github", "repo": "devzl/handover" }
    }
  },
  "enabledPlugins": {
    "handover@handover": true
  }
}
```

### Claude Code (manual skill copy)

Copy the skill folder into your personal or project skills directory:

```bash
# personal (all projects)
git clone https://github.com/devzl/handover.git /tmp/handover
cp -r /tmp/handover/plugins/handover/skills/handover ~/.claude/skills/handover

# project-local
cp -r /tmp/handover/plugins/handover/skills/handover .claude/skills/handover
```

Optionally add a shortcut command, e.g. `~/.claude/commands/handover.md`:

```markdown
Use the global `$handover` skill to generate a clean handover for the
current project and conversation.
```

Then invoke with `/handover` (or your own alias, e.g. `/h`, `/hd`).

### Cursor

Cursor supports the Agent Skills format directly: drop the folder into
your project's `.cursor/skills/` directory:

```bash
mkdir -p .cursor/skills
cp -r plugins/handover/skills/handover .cursor/skills/handover
```

### OpenAI Codex / Codex CLI

This repo also ships a Codex-compatible plugin marketplace
(`.agents/plugins/marketplace.json`), so you can add it and install the
plugin the same way as Claude Code:

```bash
codex plugin marketplace add devzl/handover
codex plugin add handover@handover
```

Or use the interactive plugin browser: run `/plugins` inside Codex CLI,
add the marketplace, then install `handover`.

Codex also reads plain skill directories directly from `.codex/skills/`
(project) or `~/.codex/skills/` (global), if you'd rather skip plugins:

```bash
mkdir -p .codex/skills
cp -r plugins/handover/skills/handover .codex/skills/handover
```

### Any other Agent-Skills-compatible tool

Copy `plugins/handover/skills/handover/` into whatever skills directory
your tool scans for `SKILL.md` files; the format is identical everywhere.
Check your tool's docs for the exact path if it isn't listed above.

### No skills support? Use it as a plain prompt

If your tool doesn't support Agent Skills at all, just paste the body of
[`plugins/handover/skills/handover/SKILL.md`](plugins/handover/skills/handover/SKILL.md)
(everything below the frontmatter) directly into your prompt or system
instructions.

## License

MIT, see [LICENSE](LICENSE).
