---
name: github-repo-audit
description: Audit and rewrite a GitHub repo's README, About sentence, topic tags, and hero asset strategy. Spawns 5 specialist research agents in parallel (competitive landscape, target-user pain language, product positioning, README visual hierarchy, SEO + AI-citation surface), synthesizes findings, drafts a severity-ranked issue list, gates on user approval, then applies fixes — rewriting README.md, updating GitHub About + topic tags via `gh repo edit`, and producing a hero-asset shotlist (handed to `claude-gif` if installed). Use whenever the user wants to audit, rewrite, reposition, or fix the public-facing surface of any open-source repo, including phrasings like "fix my readme", "rewrite my readme", "audit my github project", "my repo isn't getting stars", "open source positioning help", or pasting a repo URL and asking what's wrong with it. Also triggers on "/github-repo-audit", "repo audit", "readme audit", "repo positioning audit", "open source readme audit". Especially valuable when the repo name is legacy, when the README mixes product description with implementation detail, when competing against a SaaS incumbent and needing sharper anti-positioning, or when a human-in-the-loop project needs to distance itself from spam/automation tools in its category.
allowed-tools: Read, Write, Edit, Glob, Grep, Task, Bash(gh repo edit:*), Bash(gh repo rename:*), Bash(gh repo view:*), Bash(git remote:*), Bash(git log:*), Bash(ls:*), Bash(mkdir:*)
argument-hint: "[repo-url-or-path]"
---

# GitHub Repo Audit

Orchestrate a multi-dimensional audit of a GitHub repo's public surface (README, About, topic tags, repo name, hero assets) and apply approved fixes. The audit phase is read-only; the apply phase is gated on explicit user approval per change.

## When this skill fits

Use this skill when the user wants to improve how their repo is *perceived* — by humans browsing GitHub, by Google, and by AI engines (Perplexity, ChatGPT, Claude) summarizing the project. It is not a code audit, security audit, or test-coverage audit. It is a positioning + discoverability + first-impression audit.

Strong-fit signals:
- Repo has a working product but the README reads like commit notes or marketing fluff
- Repo name is legacy and no longer matches what the project does
- Project competes with a well-known SaaS and needs sharp anti-positioning
- Project is human-in-the-loop / responsible-tooling and risks being lumped in with spammy alternatives
- Star count or install rate feels low relative to product quality

Poor-fit signals (decline and redirect):
- User wants code review → suggest `code-reviewer`
- User wants security audit → suggest `security-review` or `cybersecurity`
- User wants Google SEO for a marketing site, not a repo → suggest `seo`

## Core principles

**Read first. Edit second. Push never (without approval).** The 5 research agents run read-only. The synthesis is a draft. Every file write and every `gh` mutation requires explicit user sign-off.

**Ground truth survives every step.** Before any agent runs, capture a 1-paragraph "what this product actually is" from the user (or extract a draft and confirm). All downstream agents receive this verbatim. If ground truth shifts mid-audit, restart the relevant agents — don't paper over it.

**Anti-positioning is a feature.** Especially for human-in-the-loop, niche, or principled tools, what the project *isn't* matters as much as what it is. The audit actively looks for opportunities to name and reject the spammy adjacent category.

**No filler. No growth-hack phrasing. No decorative emojis.** The skill enforces hard rejection criteria on the writer's output (see below).

## The 7-step flow

### Step 1 — Intake

Collect:
1. **Repo location.** GitHub URL or local path. If local, confirm the `origin` remote with `git remote -v`.
2. **Product ground truth.** One paragraph: what the product actually does, who it's for, what it explicitly is NOT, what fear/pain the user feels around the category. If the user hasn't provided this, draft it from the existing README + `package.json`/`pyproject.toml`/`Cargo.toml` + last 20 commits, then show the draft and ask: "Is this accurate? Anything I got wrong or should add?"
3. **Constraints.** Any sections the user wants preserved verbatim? Any phrasing they hate? Any competitors they want named (or avoided)?
4. **Optional: target outcome.** "More stars from solo developers" vs "enterprise inbound" vs "stop being mistaken for a spam tool" — the audit weights differently.

Do not proceed past Step 1 without ground truth. The 5 agents are useless without it.

### Step 2 — Spawn 5 agents in parallel

Issue a **single tool-call batch** with these five `Task` tool invocations (one per `subagent_type`). Running them sequentially defeats the purpose — they're independent and should overlap.

Each agent receives the ground truth verbatim + the agent-specific brief from `references/agent-briefs.md`. Read that file for the exact prompts to use.

The five agents:
1. `competitive-analyst` — Adjacent-category OSS README survey
2. `market-researcher` — Target-user pain vocabulary + objection map
3. `product-strategist` — Anti-positioning + comparison-table axes
4. `ui-ux-designer` — Above-fold blueprint + hero asset spec
5. `content-marketer` — GitHub SEO + Google SEO + AI-citation surface

### Step 3 — Synthesis

Merge the five returns into a single synthesis document. Required fields:
- **Final positioning sentence** (one sentence, no more)
- **Above-fold blueprint** (H1, sub, hero asset, anti-positioning callout, primary CTA)
- **Keyword → H2 assignment map**
- **Comparison-table rows and columns** (with the axes chosen to flatter the project's actual differentiator)
- **FAQ list** (drawn from researcher objections + buyer-intent queries)
- **Hero asset shotlist** with static end-frame notes
- **GitHub metadata** (About sentence ≤350 chars, topic tags ≤20)
- **Suggested new repo name** if and only if the current name is genuinely misleading

If two agents return contradictory recommendations, flag the contradiction in the synthesis and ask the user to resolve before applying. Don't average — pick.

### Step 4 — Issue list with severity

Convert the synthesis into a prioritized issue list. Each issue gets:
- **Severity:** Critical / High / Medium / Low
- **Where:** the part of the repo surface affected (README section, About, topic tags, repo name, hero asset)
- **What's wrong:** one sentence
- **Proposed fix:** the exact replacement text or action

Severity guide:
- **Critical** — anything that actively misleads users about what the product does or could damage trust (e.g., implies auto-publish when it's human-in-the-loop)
- **High** — first-impression failures (broken hero, no anti-positioning, legacy name, missing install)
- **Medium** — structural improvements (TOC, accordions, comparison table)
- **Low** — polish (badge cleanup, alt text, tag refinement)

### Step 5 — Approval gate

Present the issue list to the user. Default selection: all Critical + High. The user can:
- Approve as-is
- Approve a subset
- Edit any proposed fix before applying
- Reject

Do not write a single file or run a single `gh` command before this gate clears.

### Step 6 — Apply approved fixes

For each approved issue:
- **README changes** — rewrite the relevant section in the required section order (see below). Use the writer self-check before saving (see Hard Rejection Criteria).
- **GitHub About** — `gh repo edit <repo> --description "<sentence>"`
- **Topic tags** — `gh repo edit <repo> --add-topic <tag>` per tag (and `--remove-topic` for removals)
- **Repo rename** — confirm again before this one specifically. `gh repo rename <new-name>` is reversible but visible. Warn about redirect implications.

**Required README section order:**
1. Hero (H1 + one-sentence sub + hero asset + primary CTA)
2. Anti-positioning callout (what this is NOT, if applicable)
3. How it works (3 steps, max)
4. Install (3 commands, max — anything longer goes in an accordion)
5. Config
6. `<details>` accordions for technical depth (architecture, advanced config, troubleshooting)
7. Comparison table (vs the named competitor + adjacent alternatives + manual)
8. FAQ (Q&A blocks formatted for AI-engine citability)
9. Honest TOS / user-responsibility framing (if the product touches a platform with terms — LinkedIn, X, Discord, etc.)
10. License + Contributing

### Step 7 — Hero asset shotlist

Always emit the shotlist (frames, dimensions, end-frame static specs, captions). Then:
- If `claude-gif` skill is installed (check `~/.claude/skills/claude-gif/` or plugin equivalents): offer to invoke `/claude-gif` with the shotlist as the brief. Don't auto-invoke — ask first.
- If not installed: output the shotlist as a standalone spec, point the user to install `claude-gif`, and continue without it.

The skill must complete end-to-end whether or not `claude-gif` is installed.

## Hard rejection criteria (writer self-check)

Before saving any README text, the writer must self-check against these. If any trigger, rewrite:

- **Growth-hack phrasing** ("unlock", "supercharge", "10x", "game-changer", "next-level") → reject
- **Emojis not earning pixels** (decorative emojis at the start of every section header) → strip
- **More than 3 functional badges** OR any decorative badges → strip
- **Implication of auto-publish, mass outreach, pods, spam** when the product is human-in-the-loop → rewrite
- **Filler verbs** ("leverage", "empower", "transform") → rewrite with concrete verbs
- **Jargon in the first 200 words** (anything a non-technical target user wouldn't recognize) → rewrite
- **Trailing "with ❤️ by [author]" or "made with caffeine"** style signoffs → strip
- **AI-generated tells** ("In today's fast-paced world", "It's important to note", em-dash overuse) → rewrite

See `references/rejection-examples.md` for before/after examples.

## Companion skills

- **`claude-gif`** (optional) — Renders the hero asset shotlist. Skill works without it (emits spec only). Install via the `claude-gif` skill bundle.
- **`seo`** (optional) — If the project has a marketing site beyond the repo, `seo` audits that separately. This skill audits the repo surface only.
- **`code-reviewer`** (optional) — For code-level review, run separately. This skill does not look at code quality.

## Reference files

- `references/agent-briefs.md` — Exact prompts to send to each of the 5 specialist agents
- `references/rejection-examples.md` — Before/after examples for each rejection criterion
- `references/section-templates.md` — Templates for hero, anti-positioning callout, comparison table, FAQ Q&A blocks

## Output artifacts

The skill produces (and saves to `<repo>/.audit/` by default, or wherever the user specifies):
1. `synthesis.md` — full synthesis document from Step 3
2. `issues.md` — prioritized issue list from Step 4
3. `README.md` — rewritten (applied to repo root if approved)
4. `github-metadata.md` — About sentence + topic tags + suggested repo name
5. `hero-shotlist.md` — asset spec for `claude-gif` or manual rendering

## Operational rules

These cover edge cases stress-testing surfaced. They're not optional — they're the difference between a clean audit and a stuck one.

### Partial agent failure
If 1 of 5 agents fails or times out, proceed to synthesis with the remaining 4 and note the gap explicitly in `synthesis.md`. If 2+ agents fail, stop and tell the user — synthesis from 3-of-5 is unreliable, retry the failed agents instead.

### Conflict resolution between agents
The five agents see the same ground truth but reach independent conclusions. Surface a conflict to the user only when it's *load-bearing*: contradictory positioning sentences, contradictory category decisions, or contradictory recommendations on whether to rename the repo. Stylistic differences (different word choices, different example competitors) get merged by the orchestrator without user input. Don't drown the user in micro-disagreements.

### Ground-truth drift detection
If during synthesis the orchestrator notices a fact in the ground truth contradicts what agents independently found (e.g., user said "open source" but every agent treats it as SaaS), pause and reconfirm with the user before continuing. Don't paper over it.

### `gh` auth and private repos
Before Step 6, check `gh auth status`. If unauthenticated, the README write still proceeds (file-only), but About/topic-tag/rename updates emit a copy-paste block of `gh` commands for the user to run after authenticating. Private repos work the same way — the audit doesn't need API access to read the local repo, only to push metadata.

### Monorepos
Audit the **root README.md only**. If `packages/*/README.md` or similar exists, mention it in the issue list as "out of scope; re-run the skill from inside the package directory if needed." Don't recursively audit.

### Re-runs
Always fresh. The audit runs end-to-end every time. Prior `.audit/` artifacts are overwritten. If the user wants delta-only analysis, they can diff `.audit/synthesis.md` against git history manually.

### Writer self-check
The rejection criteria are applied **inline** by the orchestrator before writing each README section. Read `references/rejection-examples.md` once at synthesis time; then for each section being written, scan against the 10 patterns before saving. No separate subagent — the cost isn't worth it for a pattern-match check the orchestrator can do in context.

### Cost expectation
A full audit runs 5 parallel `Task` calls + 1 synthesis pass + 1 writer pass. On a typical OSS repo, expect $0.20–$0.50 in model spend depending on context size. Tell the user this up front if the repo is small or the audit is exploratory.

## What this skill does NOT do

- Push commits or open PRs automatically — the user does that
- Modify code outside README/metadata
- Audit security, performance, or test coverage
- Generate the actual hero GIF without `claude-gif` installed
- Promise traffic, stars, or conversion outcomes — positioning is necessary but not sufficient
