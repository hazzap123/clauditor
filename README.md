# clauditor

**A Claude Code best-practices auditor.** A Claude Code subagent that audits a Claude Code setup — `CLAUDE.md`, `settings.json`,
skills, agents, hooks, and headless `claude -p` usage — against a maintained
best-practices reference, and writes evidence-bound findings. Ships with an optional
scheduled job that keeps the reference current from Anthropic's public documentation.

## Contents

| Path | What it is |
|------|------------|
| `agents/best-practices-auditor.md` | The auditor subagent. Reads the guide (and an optional decisions file), audits your config, writes a findings file. |
| `guide/best-practices-guide.md` | The reference the auditor checks against — a cited synthesis of public Claude Code best practices. Dated; meant to be kept current. |
| `decisions/resolved-decisions.example.md` | Template for recording deliberate divergences so the auditor treats them as intentional rather than flagging them as gaps. |
| `routine/weekly-refresh.md` | Prompt and setup steps for an optional scheduled agent that diffs Anthropic's docs against the guide and opens a pull request. |

## Requirements

- Claude Code (CLI, desktop, or IDE extension).
- For the optional weekly refresh only: a claude.ai account with scheduled routines (Claude Code on the web).

## Install

1. **Copy the agent** into your agents directory:
   ```bash
   # global (applies to all projects)
   cp agents/best-practices-auditor.md ~/.claude/agents/

   # or per-project
   cp agents/best-practices-auditor.md /path/to/project/.claude/agents/
   ```

2. **Copy the guide** (and, optionally, the decisions template) into your repo — `docs/` is conventional:
   ```bash
   cp guide/best-practices-guide.md            /path/to/project/docs/
   cp decisions/resolved-decisions.example.md  /path/to/project/docs/resolved-decisions.md
   ```

3. **Set the paths.** Open `best-practices-auditor.md` and set `GUIDE_PATH` (and
   `DECISIONS_PATH`, if used) near the top to where you put those files. This is the
   only configuration step.

Project agents register immediately. A global agent is available in new sessions.

## Use

In any Claude Code session:

```
audit my Claude Code setup
```

The agent reads the guide, sweeps your config, and writes a findings file
(default `docs/auditor-findings.md`) with five sections: **Aligned**, **Gaps**,
**Divergences**, **Headless-mode findings**, and a ranked **action shortlist**.
Every finding cites a file path and a source.

It is a subagent, not a slash command — invoke it by asking, or name it explicitly:
`use the best-practices-auditor agent to audit ~/.claude`.

## Resolved decisions (optional)

A plain audit flags every deviation from the reference as a gap. Some deviations are
deliberate, justified choices. Record those in a decisions file (see
`decisions/resolved-decisions.example.md`) and point `DECISIONS_PATH` at it; the auditor
then treats them as intentional instead of flagging them. Omit it and the auditor checks
against the guide alone.

## Keep the guide current (optional)

Best-practice docs change. Instead of re-reading them, schedule an agent to do it:

1. Open `routine/weekly-refresh.md` and copy the prompt.
2. Create a routine at <https://claude.ai/code/routines> (or via the Claude Code
   `/schedule` skill), pointed at your fork, on a weekly cron.
3. It fetches Anthropic's public docs, diffs them against your guide, and opens a pull
   request with proposed updates — flagging anything that conflicts with your decisions
   file rather than overwriting it.

You review the PR.

## How it works

```
public docs ──(synthesise)──▶ guide ──(read by)──▶ auditor ──▶ findings
                  ▲                                    │
                  └──── weekly refresh (opens PR) ◀─────┘     reads: your config
                                                              + resolved decisions
```

The auditor is a standard Claude Code subagent: a Markdown file with YAML frontmatter,
whose body is the audit instructions. It reads the guide as its standard and your config
as the target.

## Limitations

- Best-practice claims are **point-in-time** and dated in the guide. Verify
  version-specific details (CLI flags, pricing, model IDs) against current docs.
- The guide **links to and paraphrases** Anthropic's public documentation; it does not
  reproduce gated course content.
- The auditor does the legwork; **judgement stays with you**. Treat findings as evidence,
  not verdicts.
- Not affiliated with or endorsed by Anthropic.

## Sources & attribution

The best practices here are derived from **Anthropic's own official tutorials, Academy
courses, and documentation**. This repository synthesises and links that public material —
it does not reproduce gated course content.

- Anthropic Academy (tutorials & courses) — <https://anthropic.skilljar.com>
- Claude Code docs — <https://code.claude.com/docs>
- Claude platform docs — <https://platform.claude.com/docs>
- Anthropic engineering blog — <https://www.anthropic.com/engineering>

## License

MIT — see [LICENSE](LICENSE).
