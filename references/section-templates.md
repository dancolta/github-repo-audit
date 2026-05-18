# Section templates

Templates for the required README sections. Adapt the bracketed slots; preserve the structure.

---

## Hero

```markdown
# [Product Name]

> [One-sentence positioning. Names the user, the action, and what makes it different from the obvious alternative.]

[![hero gif or screenshot](path/to/hero.gif)](optional-demo-link)

[Functional badges on one line — max 3]

**Install:** `[one-liner install command]` · **Docs:** [link] · **Demo:** [link]
```

The hero asset is the single most important visual decision. If using a GIF: cap at 12 seconds, ensure the static end-frame shows the *result* (not the loading state), because that's what users see when the GIF stops.

---

## Anti-positioning callout

Only include if the product has a clear adjacent category it wants to reject. Place directly after the hero.

```markdown
> **What this isn't:** [Concrete list of the spammy/adjacent thing this is not.]  
> [One sentence explaining what it is instead, naming the principle that makes it different.]
```

Example:
```markdown
> **What this isn't:** an auto-poster, a comment farm, or a pod. There is no auto-publish.  
> Every draft passes through your eyes and your click before it leaves the laptop.
```

---

## How it works (3 steps, max)

```markdown
## How it works

1. **[Step 1 verb-led]** — [one sentence, what happens, what the user sees]
2. **[Step 2 verb-led]** — [one sentence]
3. **[Step 3 verb-led]** — [one sentence, ending in the user-facing outcome]
```

If the actual flow has 7 steps, group them. Three is the maximum a scanner will absorb.

---

## Install (3 commands, max)

```markdown
## Install

```bash
[command 1]
[command 2]
[command 3]
```

That's it. [Optional one-line caveat about prereqs.]
```

If install genuinely needs more than 3 commands, the extras go inside `<details>`:

```markdown
<details>
<summary>Detailed install (custom paths, Docker, etc.)</summary>

...

</details>
```

---

## Config

```markdown
## Config

| Variable | What it does | Default |
|---|---|---|
| `KEY_1` | [purpose] | [default] |
| `KEY_2` | [purpose] | required |
```

Or — if config is simple — a single fenced `.env.example` block. Don't mix prose and table; pick one.

---

## Comparison table

```markdown
## How it compares

|  | [This Product] | [Competitor A] | [Competitor B] | Manual |
|---|:---:|:---:|:---:|:---:|
| [Axis 1 — chosen to flatter the differentiator] | ✓ | ✗ | partial | ✓ |
| [Axis 2] | ✓ | ✓ | ✗ | ✗ |
| [Axis 3] | ✓ | ✗ | ✗ | ✓ |
```

Axes are chosen by the `product-strategist` agent. Pick axes where the differentiator wins — that's the point of the table. Don't include axes where the competitor obviously wins (cost, polish, support) unless they're table stakes.

---

## FAQ (Q&A blocks for AI-engine citation)

```markdown
## FAQ

### [Question phrased exactly as a user would type it]

[Answer in 1–3 sentences. Self-contained — no "see above" or "as mentioned." Lead with the product name + the key fact in the first sentence so AI engines can cite the passage standalone.]

### [Next question]

[Answer.]
```

4–6 questions is the sweet spot. Source them from the `market-researcher` objection map + the `content-marketer` buyer-intent queries.

---

## Honest TOS / user-responsibility framing

Only include if the product touches a platform with terms of service (LinkedIn, X, Discord, Reddit, etc.).

```markdown
## A note on [platform] terms

[Platform] doesn't love automation tools — even ones with a human in the loop. [Product] is built to stay on the responsible side of that line: [specific design choices, e.g., low daily caps, no auto-publish, human approval on every action].

You are responsible for how you use it. The hard cap is [N] [actions] per day for a reason; raising it is your call and your risk.
```

This callout *builds* trust for the right user (the responsible operator) and filters out the wrong user (the spammer). Don't soften it.

---

## License + Contributing

```markdown
## License

[MIT/Apache-2.0/etc.] — see [LICENSE](LICENSE).

## Contributing

[Brief paragraph or link to CONTRIBUTING.md.]
```

Keep this section short. Detailed contributor guides go in a separate `CONTRIBUTING.md`.
