# Contributing to clauditor

Thanks for your interest. clauditor is a small, focused project: a Claude Code
subagent plus a best-practices reference it audits against. Most useful
contributions fall into a few clear buckets.

## What's most useful

1. **Keeping the guide current.** `guide/best-practices-guide.md` is a dated,
   cited synthesis of Anthropic's *public* documentation. When Anthropic ships a
   change (new Claude Code feature, changed CLI flag, new model ID, updated
   guidance), a PR that updates the relevant section — **with a source link** — is
   the single most valuable contribution.
2. **Improving the auditor.** Sharper checks, clearer findings, or better
   instructions in `agents/clauditor.md`.
3. **Fixing source links** that have rotted or moved.
4. **Docs and examples** — README clarifications, a better
   `decisions/resolved-decisions.example.md`.

## Ground rules

- **Cite your sources.** Any best-practice claim in the guide must link to public
  Anthropic material (docs, engineering blog, Academy, changelog/release notes).
  Don't reproduce gated course content — link and paraphrase.
- **Point-in-time, not gospel.** Best-practice details change. Keep claims dated
  and verifiable rather than absolute.
- **No secrets, no machine-specific paths.** Don't commit absolute home paths,
  tokens, or anything from your own setup.
- **Not affiliated with Anthropic.** Keep wording consistent with that — this is
  an independent synthesis, not an official Anthropic artifact.

## How to propose a change

1. Fork the repo and create a branch.
2. Make your change. Keep PRs small and focused — one concern per PR.
3. In the PR description, say **what changed and why**, and link the source for
   any best-practice claim.
4. Open the PR against `main`.

## Reporting issues

Use [Issues](https://github.com/hazzap123/clauditor/issues) for bugs, stale
sources, or suggestions. A good issue says what you expected, what happened, and
(for guide issues) a link to the source that contradicts the current text.

## License

By contributing, you agree your contributions are licensed under the project's
[MIT License](LICENSE).
