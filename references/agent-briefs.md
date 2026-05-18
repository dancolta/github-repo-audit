# Agent briefs

Send these prompts verbatim to each agent in the Step 2 parallel batch. Always prepend the product ground truth paragraph (captured in Step 1) to each brief so all five agents reason from the same understanding.

Spawn all five in a single tool-call batch. Do not run them sequentially.

---

## 1. competitive-analyst

**subagent_type:** `competitive-analyst`

**Prompt:**

You are auditing the README and public-facing surface of an open-source repo. Product ground truth:

> [INSERT GROUND TRUTH PARAGRAPH]

Survey 5–10 high-star OSS repositories in adjacent categories. Pick repos that share at least one of: (a) target user, (b) integration surface (Notion / Playwright / browser automation / creator tooling / social platform), (c) human-in-the-loop or principled-tooling positioning.

Return:
1. **Structural patterns correlating with stars.** What above-fold compositions repeat across high-trust READMEs? Common section orderings? TOC vs. no TOC? Accordions vs. flat?
2. **Hero asset choices.** GIF vs. static screenshot vs. terminal recording vs. architecture diagram. Dimensions, frame count, looping behavior, end-frame strategy.
3. **Badge discipline.** How many badges, which ones, where placed. Which badges signal trust vs. which feel decorative.
4. **What high-trust READMEs avoid.** Common anti-patterns absent from high-star repos — phrasing, structure, asset choices.
5. **Direct lessons.** 5 specific structural changes this repo should make based on what works in adjacent winners.

Keep returns concise (under 800 words). Cite repo names + star counts.

---

## 2. market-researcher

**subagent_type:** `market-researcher`

**Prompt:**

Profile the target user of this product. Product ground truth:

> [INSERT GROUND TRUTH PARAGRAPH]

Return:
1. **Pain vocabulary.** The exact words and phrases this target user uses to describe the pain the product solves. Source from Reddit threads, X/Twitter posts, HN comments, niche forums. Quote 5–10 verbatim snippets.
2. **Top 5 install objections.** What stops a curious visitor from running `git clone`. Order by frequency.
3. **Trust builders.** Specific signals (in copy, in assets, in metadata) that make this target user trust an OSS project enough to install it.
4. **Trust killers.** Specific signals that make them close the tab. Be concrete (e.g., "discord invite as the only support channel" > "looks unprofessional").
5. **Tone calibration.** Words that signal "thoughtful tool" vs. words that signal "spam / growth hack" to this specific audience.

Under 600 words. Quote sources where possible.

---

## 3. product-strategist

**subagent_type:** `product-strategist`

**Prompt:**

Position this product against its competitive set. Product ground truth:

> [INSERT GROUND TRUTH PARAGRAPH]

Identify the 3–5 named competitors (SaaS incumbents, OSS alternatives, manual workflows). Return:
1. **Category decision.** What category does this product belong to? What category should it actively reject?
2. **One-sentence positioning statement.** For [target user] who [pain], [product] is the [category] that [unique differentiator], unlike [primary competitor] which [anti-pattern]. Write the actual sentence, not the template.
3. **3 anti-positioning lines.** Short, sharp callouts that name the spammy adjacent category and reject it. These go above the fold.
4. **Comparison-table axes.** 6–10 row labels chosen to *flatter the actual differentiator*. Plus 3–5 column headers (this product + named competitors + "manual"). Indicate which cells should be ✓ / partial / ✗ for this product specifically.
5. **One thing the project should stop claiming.** If the current README overclaims or makes a claim that doesn't survive scrutiny, name it.

Under 500 words.

---

## 4. ui-ux-designer

**subagent_type:** `ui-ux-designer`

**Prompt:**

Design the visual hierarchy of this repo's README. Product ground truth:

> [INSERT GROUND TRUTH PARAGRAPH]

Return:
1. **Above-fold blueprint.** Exact stacking order for what appears before the user scrolls. H1, sub, hero asset placement, anti-positioning callout, primary CTA. Where each element sits relative to the others.
2. **Hero asset spec.** Recommended type (GIF / static screenshot / terminal cast / architecture diagram). Dimensions in pixels. Frame count if animated. Static end-frame requirement (what the asset shows when the GIF loops to its last frame — this is what users actually see most of the time).
3. **TOC pattern.** Should this repo use a TOC? If yes, what style (flat list, grouped, collapsed in `<details>`). If no, why not.
4. **Accordion pattern.** Which sections belong inside `<details>` accordions to keep the surface scannable. What goes in the summary line vs. inside.
5. **Badge discipline.** Maximum 3 badges, all functional (build status, license, package version — pick 3). Position: directly under the H1, single line, no decorative wrappers.
6. **Screenshot inventory.** Beyond the hero, what 2–4 additional images would clarify the product. Where each goes.

Under 600 words.

---

## 5. content-marketer

**subagent_type:** `content-marketer`

**Prompt:**

Optimize this repo for three discovery surfaces: GitHub search, Google search, and AI-engine citation. Product ground truth:

> [INSERT GROUND TRUTH PARAGRAPH]

Return three sections:

### a. GitHub SEO
- **Repo title** — exact string for the H1 (matches the repo name OR adds a tagline; pick one).
- **About sentence** — ≤350 characters, includes the primary keyword, names the differentiator, mentions the integration surface. This becomes the GitHub repo About.
- **Topic tags** — up to 20, ranked by search volume and intent fit. Mix category tags, integration tags, and "alternative-to" tags where applicable.

### b. Google SEO
A keyword map with each keyword classified by buyer intent:
- **Navigational** (user already knows the brand) — e.g., "[product name] github"
- **Comparison** — e.g., "[saas competitor] alternative open source"
- **Solution-aware** — e.g., "[capability] open source"
- **Problem-aware** — e.g., "how to [pain phrase]"
- **Long-tail variants** for each

Assign each keyword to a README H2 where the user-intent match is strongest.

### c. AI-engine citability
4–6 passage-level Q&A blocks formatted for direct citation by Perplexity, ChatGPT, and Claude. Each block:
- Question phrased as a user would ask it
- Answer in 1–3 sentences, self-contained (no "see above"), with the product name + the key fact in the first sentence

Under 700 words total.
