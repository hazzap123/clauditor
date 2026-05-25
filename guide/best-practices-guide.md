# Claude Code Best-Practices Reference

**Last reviewed:** 2026-05-25
**Status:** starter reference — keep current with the weekly refresh (`routine/weekly-refresh.md`).

A cited synthesis of public Claude Code best practices, used as the standard for the
best-practices auditor. Claims are point-in-time; verify version-specific details against
current docs. Links point to Anthropic's public documentation.

---

## 1. Architecture

- Prefer the simplest design that works. A single agent with routing handles most
  per-turn workloads; reserve multi-agent fan-out for genuinely parallelisable or
  context-exceeding work, which costs substantially more tokens.
- Distinguish *workflows* (fixed, predictable steps) from *agentic* work (model decides
  the path). Don't carry agent overhead for a workflow.
- Source: <https://www.anthropic.com/engineering/building-effective-agents>

## 2. Context management (CLAUDE.md)

- Keep `CLAUDE.md` concise. Long memory files consume context and reduce instruction
  adherence — the cost is correctness, not just tokens. Split stable detail into skills
  or referenced files.
- Put durable project facts in `CLAUDE.md`; put procedures in skills that load on demand.
- Source: <https://code.claude.com/docs/en/memory>

## 3. Skills

- A skill is a Markdown file (`SKILL.md`) with YAML frontmatter (name, description) plus a
  body that loads only when the skill is invoked — so long reference material costs little
  until needed.
- The `description` is the routing surface: lead with sharp trigger phrases / quoted user
  utterances. In headless mode, skills fire only on description-matching.
- Keep the body within the documented size guidance; split large skills into a thin router
  plus referenced resource files.
- `allowed-tools` auto-approves the listed tools while the skill is active — useful for
  unattended runs to avoid permission stalls.
- Source: <https://code.claude.com/docs/en/skills>

## 4. Subagents

- Each subagent is a Markdown file with frontmatter (name, description, tools, model) and
  a system-prompt body. Scope each to one domain.
- Set `model` per task: cheaper/faster models for lookups and triage; stronger models for
  multi-hop reasoning. Don't default everything to the most expensive model.
- Scope MCP servers in subagent frontmatter so the parent doesn't pay context tax for
  tools it doesn't use.
- `tools` is an allowlist; if set, the subagent can use only those tools.
- Source: <https://code.claude.com/docs/en/sub-agents>

## 5. MCP servers

- Tool *names* load at startup; *schemas* are deferred. Don't force-load every server's
  tools into every session — keep the startup load surface small.
- Tool description quality is a routing surface; trigger-rich descriptions improve
  selection.
- Source: <https://code.claude.com/docs/en/mcp>

## 6. Headless mode (`claude -p`)

- `claude -p` is the production runtime for external orchestrators. Anything that works
  interactively but breaks headlessly is a real risk.
- Slash commands do not work in `-p`; describe the task instead, and rely on skills firing
  via description-matching.
- Bound agentic iteration (e.g. a max-turns ceiling) to avoid runaway cost and
  "thinking-forever" failure modes.
- Use `--output-format json` if the caller parses cost/usage.
- Source: <https://code.claude.com/docs/en/headless>

## 7. Cost

- Match model to task (see §4).
- Prompt caching: place the cache breakpoint at the end of the stable prefix, not on the
  varying suffix; confirm via the API response that cache *reads* grow across turns rather
  than re-creating the cache each call.
- Bound iteration in headless mode.
- Source: <https://platform.claude.com/docs/en/docs/build-with-claude/prompt-caching>

## 8. Security red-lines

- `--dangerously-skip-permissions` is appropriate only for trusted, filtered invocations;
  flag other uses.
- No secrets in tracked files or inherited env passed to subprocesses.

---

## Refresh log

- 2026-05-25: initial starter reference compiled from public Anthropic documentation.
