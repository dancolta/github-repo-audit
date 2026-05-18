# github-repo-audit

Claude Code skill that audits a GitHub repo's README, positioning, and discoverability — then applies the fixes.

> **What this isn't:** a code linter, a security scanner, or a vanity star-counter. It looks at how your repo is *perceived* — by humans skimming GitHub, by Google, and by AI engines summarizing your project. Nothing else.

## How it works

1. **Intake** — you paste a repo URL (or point at a local path) and a one-paragraph "what this product actually is." If you skip that paragraph, the skill drafts one from your README and asks you to confirm.
2. **Five agents in parallel** — competitive-analyst, market-researcher, product-strategist, ui-ux-designer, content-marketer. Each looks at one dimension of your repo's public surface and returns under 800 words.
3. **You approve, the skill applies** — synthesis becomes a severity-ranked issue list. You pick what to apply. Nothing writes to disk or hits `gh repo edit` without your sign-off.

## Install

```bash
mkdir -p ~/.claude/skills && \
  git clone https://github.com/dancolta/github-repo-audit ~/.claude/skills/github-repo-audit
```

That's it. The skill is now available as `/github-repo-audit` (and triggers on phrases like "audit my repo" or "rewrite my readme").

## Usage

```
/github-repo-audit https://github.com/yourname/yourproject
```

Then answer the ground-truth question, watch the 5 agents run in parallel, review the issue list, and approve the fixes you want applied.

## What it produces

- `synthesis.md` — combined findings from all 5 agents
- `issues.md` — Critical/High/Medium/Low issues with proposed fixes
- `README.md` — rewritten in a fixed section order (Hero → Anti-positioning → How it works → Install → Config → Accordions → Comparison → FAQ → TOS → License)
- `github-metadata.md` — About sentence + topic tags + suggested rename
- `hero-shotlist.md` — frame-by-frame spec for the hero asset

## Companion skill

[`claude-gif`](https://github.com/dancolta/claude-gif) — optional. If installed, the audit offers to render the hero asset shotlist into an actual GIF. The audit works end-to-end without it; you just get the spec instead of the file.

## How it compares

|  | github-repo-audit | Manual rewrite | Generic LLM prompt |
|---|:---:|:---:|:---:|
| Multi-dimensional analysis (5 specialist lenses) | ✓ | partial | ✗ |
| Source-cited competitive patterns | ✓ | depends | ✗ |
| Severity-ranked issue list before any changes | ✓ | ✗ | ✗ |
| Applies fixes (README + `gh` metadata) | ✓ | manual | ✗ |
| Hero asset shotlist with end-frame specs | ✓ | rare | ✗ |
| Approval gate on every change | ✓ | n/a | depends |

## FAQ

### Does this skill work on private repos?

Yes. The audit reads the local repo and works offline for the README. The `gh repo edit` step needs `gh auth status` to pass; if you're unauthenticated, the skill emits a copy-paste block of the commands instead of running them.

### What if the 5 agents disagree?

The orchestrator surfaces a conflict to you only when it's load-bearing — contradictory positioning sentences, category decisions, or rename recommendations. Stylistic differences get merged automatically. You're not asked to arbitrate every micro-disagreement.

### Will it overwrite my README without warning?

No. Every file write and every `gh` mutation requires your explicit approval. The 5 research agents run read-only.

### Does it work on monorepos?

It audits the root README. Sub-package READMEs (`packages/*/README.md`) are out of scope — re-run the skill from inside the package directory if you need those audited.

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built by [Dan Colta](https://github.com/dancolta) as a Claude Code skill. Bug reports: [issues](https://github.com/dancolta/github-repo-audit/issues).
