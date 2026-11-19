# jt-dev-loop

Reusable Claude Code development loop extracted from the workflow developed for Haunted Halls.

It packages:

- a generic implementation/review/remediation skill;
- an independent **high-effort** reviewer;
- a **high-effort** Director that gates findings, coverage, and readiness;
- restart-safe PR review state;
- one targeted re-review per exact head;
- external GitHub review-evidence reconciliation;
- end-to-end/runtime reachability checks;
- optional project-specific policy through `.claude/dev-loop-policy.md`;
- a local/non-PR mode for take-home or early-stage work.

The plugin never merges autonomously. It stops at `READY FOR HUMAN VALIDATION`.

## Layout

```text
.claude-plugin/
  plugin.json
skills/
  dev-loop/
    SKILL.md
agents/
  reviewer.md
  director.md
docs/
  project-policy-contract.md
examples/
  haunted-halls-policy.md
```

## Test from a clone

Clone this repository, then validate it:

```bash
claude plugin validate .
```

Run one Claude Code session with the checkout loaded:

```bash
claude --plugin-dir /path/to/jt-dev-loop
```

Invoke the loop explicitly with:

```text
/jt-dev-loop:dev-loop
```

Plugin subagents appear under scoped names:
- `jt-dev-loop:reviewer`
- `jt-dev-loop:director`

## Make it available in every local project

Claude Code auto-loads plugins stored as plugin directories under `~/.claude/skills/`. A convenient setup is to make the Git checkout itself the installed copy:

```bash
git clone git@github.com:jtesolin/jt-dev-loop.git ~/.claude/skills/jt-dev-loop
claude plugin validate ~/.claude/skills/jt-dev-loop
```

Then start a new Claude Code session (or use `/reload-plugins` when applicable). Future updates are simply:

```bash
cd ~/.claude/skills/jt-dev-loop
git pull
```

This keeps GitHub as the source of truth and makes recovery on a new machine straightforward.

## Project-specific behavior

The plugin is deliberately generic. Put project-only architecture and completion requirements in:

```text
.claude/dev-loop-policy.md
```

See [docs/project-policy-contract.md](docs/project-policy-contract.md) for the contract and [examples/haunted-halls-policy.md](examples/haunted-halls-policy.md) for an example.

Normal project `CLAUDE.md` / `AGENTS.md` instructions still matter. During PR review, the loop uses trusted-base versions so a change cannot redefine the rules used to approve itself.

## Effort model

The implementation session retains whatever effort you selected for the session.

The plugin fixes both independent control agents at:

```yaml
model: inherit
effort: high
omitClaudeMd: true
```

This deliberately decouples implementation cost from review/governance scrutiny.

## Status

Initial generic extraction. The Haunted Halls repositories still contain their existing project-local loop while this plugin is tested. Migration should happen only after the generic plugin has been exercised successfully.
