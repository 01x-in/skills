# Field Guide — What Each Agent Needs

This file defines "specific enough" for each product-seed.md field.
Read this during Step 2 scoring to decide whether a field needs clarification.

---

## Problem Statement

**What system-design-agent uses it for:**
Infers scale, data sensitivity, integration requirements, and performance targets.
A vague problem statement produces generic architecture choices.

**What product-brief-agent uses it for:**
Derives the "frustration" section of each persona. Weak problem statements
produce personas that don't feel real.

**Scores:**
- 3: Names a specific pain with a specific trigger. "Ops teams spend 40 mins manually
  cross-referencing Jira tickets with Confluence docs before every standup" — agent can
  infer: async document reads, moderate data volume, internal tool, no public auth.
- 2: Pain is clear but trigger is vague. "Teams waste time on status updates" — workable
  but will produce a slightly generic system design.
- 1: Describes a solution, not a pain. "A tool to help teams communicate better" — ask.

**What to ask when score is 1:**
"What's the specific moment the user hits this pain? What are they doing right before
they feel it?"

---

## Target User

**What milestone-agent uses it for:**
Scopes Milestone 1 around the primary user's most critical flow. A vague target user
produces a milestone plan that tries to serve everyone and ships nothing useful.

**What user-stories-agent uses it for:**
Writes the "As a..." framing and generates realistic edge cases. Vague users produce
generic edge cases that miss the real failure modes.

**Scores:**
- 3: One person, one situation, one goal. "A freelance designer who just finished a
  project and needs to send an invoice but doesn't use accounting software" — complete.
- 2: Role is clear but situation is missing. "Freelance designers" — agent can infer
  but may miss the key trigger moment.
- 1: Too broad to act on. "Anyone who needs to manage files" or "small businesses" — ask.

**What to ask when score is 1:**
"Describe the exact moment this person reaches for your product. What just happened?"

---

## Core Value Proposition

**What product-brief-agent uses it for:**
The opening line of the product positioning and the lens for every UX principle.

**What review-agent uses it for:**
Cross-checks that every feature in the plan serves the stated value prop.
A vague value prop makes this check impossible.

**Scores:**
- 3: One sentence, specific outcome, specific user. "PerishNote lets anyone share a
  self-destructing checklist with a password — no signup, no trace."
- 2: Two sentences, or missing the "without [pain]" half. Can be compressed.
- 1: Describes the category, not the differentiation. "A tool for team communication." Ask.

**What to ask when score is 1:**
"In one sentence: what does your product let the user do that they couldn't easily do before?"

---

## Key Features

**What milestone-agent uses it for:**
Directly maps features to stories and milestones. Vague features produce overlapping
stories and unclear definitions of done.

**What user-stories-agent uses it for:**
One story per feature (roughly). Vague features produce vague acceptance criteria.

**What system-design-agent uses it for:**
Derives API surface and data model from features. "Dashboard" tells the agent nothing.
"Real-time dashboard showing active sessions per tenant with 60s refresh" tells it everything.

**Scores:**
- 3: Written as user capabilities, specific enough to test. "User can create a
  password-protected list and get a shareable URL" — testable, clear scope.
- 2: Feature exists but scope is fuzzy. "Sharing" — workable but will produce loose stories.
- 1: Category placeholder. "Analytics", "Dashboard", "Admin panel" — ask what it shows and why.

**What to ask when score is 1:**
"For [vague feature] — what specifically can the user see or do? What triggers them to open it?"

---

## Tech Preferences

**What system-design-agent uses it for:**
This is the one field where "no preference" is a valid and complete answer.
If blank, the agent makes sensible decisions. If partially filled, the agent
respects the constraints and fills the gaps.

**Scores:**
- 3: Named stack with reason or constraint. "Next.js on Vercel — must deploy there,
  existing account." or "No preference — leave to agent."
- 2: Framework named, hosting unknown. Still workable.
- 1: Contradictory preferences. "Django but also serverless" — clarify before writing.

**What to ask when score is 1:**
"Do you have a hosting provider or framework you're locked into, or is the stack open?"

---

## Constraints

**What system-design-agent uses it for:**
The most architecturally impactful field. A constraint like "no user accounts" or
"must work offline" changes the entire system design. These cannot be inferred.

**What architect-agent uses it for:**
Determines what infra to provision and what to leave for the human.

**Scores:**
- 3: Specific and testable. "Must work on mobile browser with no app install."
  "HIPAA-compliant — no PHI in logs." "Zero user accounts — anonymous access only."
- 2: Implied but not stated. If the conversation strongly implies mobile-first, score 2.
- 1: Nothing mentioned. For most products this is fine — write "None stated."
  Only ask if the product domain implies hidden constraints (healthcare → HIPAA,
  fintech → PCI, EU users → GDPR).

---

## Out of Scope

**What milestone-agent uses it for:**
The most underrated field. milestone-agent uses out-of-scope to bound Milestone 1.
An empty field causes the agent to speculate about what's included and produces
overloaded first milestones.

**What review-agent uses it for:**
Flags if any planning doc includes features that were explicitly excluded here.

**Scores:**
- 3: Named exclusions that a reasonable person might expect to be included.
  "No user accounts or email auth — anonymous only."
  "No mobile app — web only."
  "No payment processing in MVP."
- 2: A few exclusions but probably more exist. Prompt the user: "What else should
  the agents not build?"
- 1: Completely empty. Always ask — even "no strong opinions" is a valid answer that
  tells the agents to use their judgment.

**What to ask when score is 1:**
"What should the agents definitely NOT build? Even obvious things are worth listing."

---

## Additional Context

**What all agents use it for:**
Anything that doesn't fit the structured fields but would affect decisions.
Competitors named here let product-brief-agent write sharper differentiation.
Known risks let system-design-agent add defensive architecture.

**Scores:**
- 3: At least one competitor or inspiration product named. At least one known risk.
- 2: One or the other.
- 1: Empty. This is acceptable — do not ask unless the conversation mentioned something
  that clearly belongs here but wasn't captured above.

---

## Design Direction

**What design-spec-agent uses it for:**
This is its primary input. The Design Direction field is the difference between
design-spec-agent producing a generic dark SaaS palette vs a precise, opinionated
token system that actually reflects the product's personality. Vague direction = generic
output. Specific direction = a design spec that build-agent can implement without
making aesthetic decisions itself.

**What build-agent uses it for (indirectly):**
build-agent reads design-spec.md, which is derived from this field. The cleaner the
direction here, the less creative latitude build-agent takes when implementing UI —
which means fewer "looks fine but feels wrong" outputs.

**What product-brief-agent uses it for:**
Cross-checks UX principles against the design direction. A product that says
"zero friction" in its brief but "dense and keyboard-driven" in design direction has
a useful tension — the brief agent should address it, not paper over it.

**Scores:**
- 3: Covers mood, palette direction, typography character, density, motion stance, and
  at least one reference product or explicit anti-reference.
  "Vercel-dashboard-like — dark, very dense, monospace for values, no decorative animation.
  Should NOT feel like a consumer app. Terse error messages."
- 2: Mood is clear but specifics are missing. "Dark and minimal, developer-focused" —
  design-spec-agent can work with this but will make more independent decisions.
- 1: Nothing mentioned, or only "looks good" / "clean" / "modern". Must ask OR
  explicitly write "No strong opinion" so design-spec-agent knows it has full latitude.

**Critical rule:**
"Clean", "modern", "minimal", and "professional" are NOT design directions.
They are the absence of a direction. If the user used these words and only these words,
score 1 and ask for something more specific.

**What to ask when score is 1:**
"What should this product feel like visually? Name one product it should feel like
(or explicitly not feel like), and whether you lean dark or light, dense or spacious."

**Design Direction → design-spec-agent output mapping:**

| Seed says | design-spec-agent produces |
|---|---|
| "Linear-like, dark, monospace" | Dark neutral palette (#0a0a0a bg), JetBrains Mono for data, Inter for prose, 2px border radius max, no shadows |
| "Premium consumer iOS" | Light warm palette, SF Pro-style geometry, 16px+ border radius, layered shadows, spring animations |
| "Brutalist, raw, anti-design" | High contrast B&W, system fonts only, no border radius, no transitions, borders instead of shadows |
| "No strong opinion" | Agent derives from product type, target user, and comp set — documents its own reasoning |

---

## Agent Handoff Note — design-spec-agent

Include the design-spec-agent block in the handoff note only when the user was
explicit about something that must not be overridden by agent aesthetic judgment.

**Include when:**
- User named a specific reference product ("make it feel like Raycast")
- User stated a hard constraint ("must be WCAG AA", "no animations — users with motion sensitivity")
- User rejected a specific aesthetic ("not bubbly, not purple, not startup-generic")
- User gave specific palette or font decisions ("I already have brand colors: #1a1a2e and #e94560")

**Omit entirely when:**
- User gave no design opinion at all — let design-spec-agent work freely
- User only used generic words like "clean" or "modern" — these go in the seed field
  itself as "No strong opinion" and the agent takes it from there

**Example of a strong design-spec-agent handoff note:**
```
design-spec-agent: User was explicit on three points:
  1. Dark mode only — no light mode, not even in post-MVP
  2. Feels like Linear or Raycast — dense, precise, keyboard-first
  3. Hard no on rounded corners and drop shadows — "too SaaS-y"
  Brand colors not decided — full latitude on palette within dark constraints.
```