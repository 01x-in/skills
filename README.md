# 01x Skills

> Skills crafted by AI builders at **[01x.in](https://01x.in)** — a paid builder environment where you join a cohort, explore an idea, use AI to accelerate, and ship an MVP.

This repo is a growing collection of Claude Code skills built from real patterns we use with our builders. Each skill lives in its own folder and can be dropped into any Claude Code project independently.

---

## Repo Structure

Every skill follows the same convention:

```
skills/
└── <skill-name>/
    ├── SKILL.md              ← skill definition (frontmatter + instructions)
    └── references/           ← supporting docs the skill reads on demand
        └── *.md
```

Adding a new skill means adding a new folder under `skills/` — nothing else needs to change.

---

## Skills

### product-seed

Turns a product ideation conversation into a build-ready `product-seed.md` — the input document for the [create-01x-project](https://github.com/01x-in/create-01x-project) AI agent build system.

`product-seed.md` is the single source of truth five specialist agents read to independently produce a system design, milestone plan, user stories, product brief, and design specification.

The seed is not a form you fill out — it's the output of a real ideation conversation. You think through your idea, pressure-test it, research competitors, debate tradeoffs, and when it's fully chalked out, generate the seed.

**Two ways to generate one:**

1. **Claude Code skill** — [skills/product-seed/SKILL.md](skills/product-seed/SKILL.md). Drop this repo (or just the `product-seed` folder) into your Claude Code skills, and it triggers automatically once an ideation conversation reaches a natural conclusion, or when you say "generate the seed".

2. **Universal prompt** — [prompt/product-seed-prompt.md](prompt/product-seed-prompt.md). For ideation happening anywhere else — ChatGPT, Gemini, Perplexity, any AI chat UI. Paste the whole file at the *end* of your ideation conversation in that tool, and it walks the conversation through the same extraction and scoring process to produce the same `product-seed.md`.

Either path produces the same nine-field seed:

| Field | Purpose |
|---|---|
| Problem Statement | The specific pain. Informs architecture scale and data model. |
| Target User | One person in one situation. Scopes Milestone 1. |
| Core Value Proposition | One sentence. The lens for every planning decision. |
| Key Features | Day-one capabilities. Maps to stories and milestones. |
| Tech Preferences | Stack decisions. Respected by the system design agent. |
| Constraints | Non-negotiables that would change the architecture. |
| Out of Scope | Bounds Milestone 1. Prevents feature creep in planning. |
| Additional Context | Competitors, risks, market context — captured before it's lost. |
| Design Direction | Visual personality. Feeds the design spec agent directly. |

The skill's only job is to produce `product-seed.md` — it doesn't check or care whether `create-01x-project` is installed. What you do with the seed afterward (scaffold a new project, drop it into an existing one, hand it to a human) is up to you.

---

*From zero to one to scale.*
