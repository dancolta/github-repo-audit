# Rejection criteria — before/after examples

Use these as a calibration set when the writer self-checks output before saving. If any of these patterns appear, rewrite.

---

## 1. Growth-hack phrasing

**Before:** "Supercharge your LinkedIn presence and 10x your engagement with AI-powered automation."

**After:** "Drafts LinkedIn comments in your voice. You approve every one before it posts. Hard cap: 15 a day."

Why: Specific, falsifiable, and names the constraint that defines the product.

---

## 2. Decorative emojis

**Before:**
```
## 🚀 Features
## 💡 How it works
## 📦 Installation
## 🎯 Why use this
```

**After:**
```
## Features
## How it works
## Install
## Why use this
```

Why: Section headers are already visually distinct. Emojis at every header add noise without information. The exception: one emoji in the H1 if it's part of the project's actual logo/identity.

---

## 3. Badge inflation

**Before:** 8 badges — build, license, version, downloads, contributors, code style, made-with-love, twitter-follow, discord-invite, sponsors.

**After:** 3 badges max — pick from: build status, license, package version. Place on one line directly under the H1. Skip the rest. Discord/Twitter/sponsors go in a footer link section, not as badges.

Why: Badge soup signals "the README was assembled, not written." High-trust repos use 0–3 badges. Decorative badges actively reduce trust.

---

## 4. Auto-publish implication for human-in-the-loop tools

**Before:** "Automate your LinkedIn engagement. Set it and forget it — the bot handles everything."

**After:** "Drafts comments to a Notion queue. You read, edit, and approve each one. Nothing publishes without your click."

Why: Even one phrase that implies auto-publish destroys trust for the exact users this kind of tool needs (founders building a brand, who fear bans and bot-sounding output).

---

## 5. Filler verbs

**Before:** "Leverage cutting-edge AI to empower creators and transform their workflow."

**After:** "Writes a draft comment in your voice. Queues it. You ship it."

Why: "Leverage", "empower", "transform" are AI-detector bait and say nothing. Replace with the concrete action.

---

## 6. Jargon in the first 200 words

**Before:** "A multi-agent, MCP-orchestrated, Playwright-driven, vector-embedded, RAG-augmented comment generation pipeline."

**After:** "Reads your LinkedIn feed. Drafts comments in your voice. Saves them to Notion for your review."

Why: The target user (non-technical operator) bounces. Technical depth goes in `<details>` accordions further down.

---

## 7. Trailing signoffs

**Before:** "Made with ❤️ and ☕ by [author]. If you like this project, give it a star! ⭐"

**After:** (remove entirely, or replace with a one-line `## Credits` section: "Built by [author]. Bug reports: [issues link].")

Why: Signoffs read as performative. A simple credit line does the same job without the cringe.

---

## 8. AI-generated tells

**Before:** "In today's fast-paced world of social media — where every interaction counts — it's important to note that authentic engagement remains paramount."

**After:** "LinkedIn rewards real comments. Bots get throttled. This tool helps you write real comments faster, not fake ones at scale."

Why: AI-tells (em-dash sandwiches, "in today's", "it's important to note", "paramount", "delve") flag the README as low-effort. The replacement is shorter, more specific, and names the actual tradeoff.

---

## 9. Vague comparison-table cells

**Before:** Comparison row: "AI quality: ⭐⭐⭐⭐ vs. ⭐⭐⭐"

**After:** Comparison row: "Voice match: drafts in your prior-comment style ✓ | uses generic templates ✗"

Why: Star ratings in comparison tables are unsubstantiated. Name the actual mechanism difference.

---

## 10. CTA chains

**Before:**
```
[![Star](badge)](link) [![Sponsor](badge)](link) [![Follow](badge)](link)
👉 Star this repo! 👉 Join our Discord! 👉 Follow on Twitter!
```

**After:** One primary CTA above the fold (usually "Install" or "See the demo"). Everything else lives in the footer.

Why: Multiple CTAs above the fold = no CTA. Pick the one action that matters and make it the only one visible.
