---
name: product-seed
description: Converts any product ideation conversation into a structured product-seed.md for the create-01x-project agent system. Use this skill whenever the user has been discussing a product idea, SaaS concept, tool, platform, or app and wants to start building — especially when they say things like "let's build this", "generate the seed", "create the product seed", "scaffold this", "let's start coding", "write up what we discussed", "convert this to a seed", or after a long ideation conversation where an idea has been chalked out. Also trigger when the user drops a product idea cold and asks to scaffold it directly. This skill deeply understands what each of the 5 downstream planning agents (system-design, milestone, user-stories, product-brief, design-spec) needs in order to produce quality output — and writes the seed to serve all five simultaneously.
---

# Product Seed Skill

Your job is to synthesise a full ideation conversation into a `product-seed.md` that is
precise enough for five specialist agents to independently produce high-quality planning
documents without asking a single clarifying question.

The seed is the **single source of truth** for the entire create-01x-project build system.
Every ambiguity you leave in the seed will produce drift between the 5 planning docs —
and the review agent will catch it, forcing a re-run. Get it right here.

---

## Step 1 — Extract from the Conversation

Read the entire conversation. Do not ask the user to repeat themselves.
Extract and categorise every signal into one of the 9 seed fields.

Use this extraction lens per field:

| Field | What to look for |
|---|---|
| Problem Statement | Pain language: "annoying", "no good tool", "always have to", "takes too long", "nobody solves" |
| Target User | Who the user keeps referring to. Specific roles, situations, behaviours — not demographics |
| Value Proposition | The "so that" — the outcome the user cares about, not the feature |
| Key Features | Everything described as must-have, core, MVP, day-one. Exclude "maybe later" |
| Tech Preferences | Any mention of stack, language, hosting, DB, framework — even casual |
| Constraints | Anything described as non-negotiable: compliance, latency, cost, mobile-first, offline, no auth, etc. |
| Out of Scope | Anything explicitly excluded, deprioritised, or deferred in the conversation |
| Additional Context | Competitors mentioned, inspiration products, prior art, known risks, market context |
| Design Direction | Aesthetic references, visual mood, density, tone, what it should feel like, what to avoid |

---

## Step 2 — Score Each Field

Before writing anything, internally score each field 1–3:

- **3** — Specific enough for an agent to act on without asking questions
- **2** — Present but vague; can be inferred with reasonable confidence
- **1** — Missing or too thin; needs clarification before the seed can be written

Any field scoring **1** is a gap. List all gaps and ask the user to fill them in a
**single consolidated question** — never ask one gap at a time, never ask for information
already in the conversation.

Read `references/field-guide.md` for exactly what "specific enough" means per field and
what the downstream agents need from each one.

---

## Step 3 — Fill Gaps (if any)

Present the gaps as a numbered list. Be direct — tell the user what you found and what
you need, referencing what they already said where relevant.

Example:
```
I pulled everything from our conversation. Strong on features and constraints.
Two gaps before I can write the seed:

1. Target user — you mentioned "busy ops teams" but I need one more degree of
   specificity. What's the trigger moment? When exactly does this user reach for the tool?

2. Design direction — nothing explicit. Should this feel like a serious developer
   tool (dense, dark, monospace) or something more consumer/approachable? Any products
   it should feel like — or explicitly not feel like?
```

Wait for the user's response before writing the seed.

---

## Step 4 — Write the Seed

Write the complete `product-seed.md` using the exact format below.
Do not add extra sections. Do not omit sections. Do not use placeholder language like
"[to be determined]" — if something is genuinely unknown, make a reasonable decision
and note it in Additional Context.

~~~markdown
# Product Seed — [Product Name]

> [One sentence that captures what this is and who it's for.]

---

## Problem Statement
[2–4 sentences. The specific pain that exists in the world without this product.
Written from the user's perspective, not the builder's. Concrete and falsifiable —
someone reading this should be able to immediately recognise the pain or say "that's not my problem".]

## Target User
[One specific type of person in one specific situation. Not a demographic.
A real human in a real moment: "A solo founder reviewing their first legal contract at 11pm
with no lawyer on call" not "small business owners".]

## Core Value Proposition
[One sentence. What this product does and why it matters to the target user.
Format: "[Product] lets [user] [do X] without [the current pain]".]

## Key Features
[Every feature the product must have on day one. One line each.
Written as capabilities ("User can X"), not technical tasks ("Build X endpoint").
No vague items like "dashboard" — specify what the dashboard shows and why.]
-
-
-

## Tech Preferences
[Stack, language, framework, hosting, DB, auth approach — anything decided in the
conversation. If none, write: "No strong preferences — leave to system-design agent."]
-

## Constraints
[Non-negotiable requirements that would change the architecture if violated.
Examples: must work offline, HIPAA-compliant, sub-200ms response, no user accounts,
mobile-first, single-tenant, must deploy to existing EKS cluster.]
-

## Out of Scope
[What this product explicitly will NOT do in the MVP. Be specific —
"no notifications" is better than "keep it simple".]
-

## Additional Context
[Competitors or inspiration products mentioned. Known risks or unknowns.
Market context. Any decisions made during ideation that don't fit above.
Architecture decisions the user was opinionated about.]

## Design Direction
[The visual and interactive personality of this product. Be specific and opinionated.
Cover: overall aesthetic mood, color palette direction, typography character, density
feel, motion philosophy, and microcopy tone. Reference real products the user named.
State what this should explicitly NOT look like.

Examples of a good Design Direction:
- "Linear-like density and precision. Dark by default, no light mode in MVP. Monospace
  for data, sans-serif for prose. Flat and sharp — no rounded corners, no drop shadows.
  Motion: state changes only, never decorative. Microcopy: terse and technical."
- "Premium consumer iOS feel — light, airy, generous whitespace. Warm off-white
  background, one coral accent. Rounded everywhere. Friendly, encouraging microcopy.
  Smooth transitions on every meaningful interaction."
- "No strong opinion on aesthetics — full creative latitude to design-spec-agent.
  Hard constraint: WCAG AA compliant, must work on small screens."]
~~~

---

## Step 5 — Annotate for Agent Clarity

After writing the seed, add a short **Agent Handoff Note** as a comment block at the
very bottom (this is read by the orchestrator, not shown in the final doc):

~~~markdown
---
<!-- Agent Handoff Note

system-design-agent: Pay close attention to [specific constraint or decision].
  The user was explicit that [X] — do not deviate.

milestone-agent: The user wants [core flow] in Milestone 1 at minimum.
  [Feature Y] was explicitly deferred to post-MVP.

user-stories-agent: Key edge cases surfaced in ideation:
  - [Edge case 1]
  - [Edge case 2]

product-brief-agent: Positioning angle = [specific framing].
  The user explicitly compared this to [competitor] and said [differentiation].

design-spec-agent: [Explicit visual or interaction decisions from ideation that must
  not be overridden — named reference products, palette constraints, typography
  requirements, density/spacing opinions, accessibility requirements.
  Omit this block entirely if the user gave no specific design direction.]
-->
~~~

Only include items where there was a specific signal in the conversation.
If the user said nothing specific about design, omit design-spec-agent's block
entirely — let the agent derive freely from the product's personality and brief.

---

## Quality Rules

- **Specificity over completeness** — a seed with 5 specific features beats one with
  10 vague ones. The agents can infer; they cannot un-confuse ambiguity.
- **No hedging language** — never write "possibly", "maybe", "could include". The seed
  must read as decisions, not options.
- **No technical implementation** — this is what, not how. "Real-time sync" yes;
  "WebSocket server with Redis pub/sub" no (that's for system-design-agent).
- **Out of scope is as important as in scope** — milestone-agent uses it to bound
  Milestone 1. An empty out-of-scope section produces bloated milestone plans.
- **The value prop must be one sentence** — if you can't write it in one sentence,
  the product isn't clear enough yet. Ask.
- **Design Direction must have a stance** — "clean and modern" is not a stance.
  "Figma-like density, neutral palette, no decorative motion" is a stance. If the user
  gave no direction, write "No strong opinion — full creative latitude to design-spec-agent"
  so the agent knows it can work freely.

---

## Output

Save the file as `agent_docs/product-seed.md` if a filesystem is available.
Otherwise present it in a code block clearly labelled as the final seed.

After the seed, print exactly:

```
Seed complete. To build:

  mkdir [project-name] && cd [project-name]
  npx create-01x-project

Drop this seed into agent_docs/product-seed.md, then:

  Run the orchestrator agent.
```