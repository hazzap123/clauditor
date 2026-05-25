# Weekly guide refresh (optional)

A scheduled agent that keeps `guide/best-practices-guide.md` current from Anthropic's
public documentation, so you review a pull request instead of tracking the docs yourself.

## Set it up

1. Fork or clone this repo so the routine has somewhere to open a PR.
2. Go to <https://claude.ai/code/routines> (or use the Claude Code `/schedule` skill).
3. Create a routine:
   - **Schedule:** weekly (e.g. `0 15 * * 5` — Friday 15:00 UTC).
   - **Repository:** your fork.
   - **Tools:** `Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch`.
   - **Prompt:** paste the prompt below.

## Prompt

```
You are a weekly maintenance agent in a fresh checkout of this repository. You start
with zero prior context — this prompt is your full brief. Goal: detect changes in
Anthropic's PUBLIC Claude Code / platform documentation since the reference guide was
last reviewed, and propose updates to it via a pull request.

REFERENCE FILE (read in full first): guide/best-practices-guide.md — learn its structure,
the sources/URLs it cites, and its "Last reviewed" date.

If a file recording deliberate decisions exists in the repo (e.g.
docs/resolved-decisions.md or decisions/), read it. Do NOT propose changes that
contradict a recorded decision; flag any conflict in the PR description for human review.

SOURCES TO CHECK (fetch current state; cite the exact URL for every proposed change):

PRIMARY — the changelog / release-notes feeds (read entries dated AFTER the guide's
"Last reviewed" date; this is the low-noise signal of what actually changed):
- Claude Code changelog — https://code.claude.com/docs/en/changelog
- Platform / API release notes — https://platform.claude.com/docs/en/release-notes/overview
- Help Center release notes — https://support.claude.com/en/articles/12138966-release-notes
- Anthropic news (model launches, pricing) — https://www.anthropic.com/news

SECONDARY — full doc pages, for context on anything the changelog flags, and a deeper
sweep roughly monthly (changelogs can miss silently-reworded conceptual guidance):
- Claude Code docs — https://code.claude.com/docs (skills, subagents, headless claude -p,
  hooks, context window, MCP, settings)
- Platform docs — https://platform.claude.com/docs (prompt caching, models, pricing)
- Anthropic engineering blog — https://www.anthropic.com/engineering

METHOD:
- Compare each source against what the guide states. Classify findings as NEW (covered
  nowhere), CHANGED (guide says X, source now says Y), or DEPRECATED (guide cites
  something no longer present).
- Be conservative and evidence-bound. Every proposed change cites the exact URL. If a
  source is unreachable, say so — never invent content. Keep edits minimal and surgical.

OUTPUT:
- Work on a branch `guide-refresh-<YYYY-MM-DD>` (get the date with `date -u +%F`). Never
  push to main; never merge.
- Update the guide's "Last reviewed" date and add a dated entry to its "Refresh log".
- If no material changes: log "checked, no material changes", commit the branch, open a PR
  titled "Guide refresh <date> — no changes".
- If changes found: update the relevant sections (cite URLs inline), then open a PR titled
  "Guide refresh <date>" whose description lists each change as
  [NEW|CHANGED|DEPRECATED] — what — source URL. Flag any conflict with a recorded decision
  in a dedicated section; do not auto-resolve it.
- End with a summary: branch name, PR URL, counts of NEW/CHANGED/DEPRECATED, and any
  sources you could not access.
```

## What you get

A weekly pull request: either "no changes" or proposed edits with cited sources. You
review and merge. The guide stays current without you tracking the docs by hand.
