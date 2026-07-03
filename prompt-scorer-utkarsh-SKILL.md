---
name: prompt-scorer-utkarsh
description: >
  Score and improve any prompt using the six-block framework (Role, Task, Context, Requirements,
  Boundaries, Reasoning), AND steer toward the right prompt tier and the right AI persona
  (Assistant / Creator / Strategist) for the task at hand. Use this skill whenever Utkarsh asks to
  "score my prompt", "grade this prompt", "rate this prompt", "improve my prompt", "make this prompt
  better", "what's wrong with this prompt", "is this a good prompt", or shares a prompt and wants
  feedback. Also trigger when he asks whether a prompt is Basic / Focused / Comprehensive, which
  persona or mode to use, "how should I ask AI to do X", or simply describes a goal or task without a
  prompt yet (e.g. building a test, article, work deliverable, or his son's practice materials) — in
  which case the skill nudges him to the right persona and tier and drafts the prompt for him. The
  skill grades each block Success / Partial / Missing, names the persona and tier, flags the
  attachment/fabrication risk, and returns an improved rewrite.
---

# Prompt Scorer & Improver

This skill does two jobs. When given a prompt, it grades it against six building blocks, names the tier and
the right AI persona, and returns a concrete improved version. When given only a *goal* — a task with no
prompt yet — it steers first: it names the persona and tier the task calls for and drafts a right-sized
prompt. It encodes the same rubric taught in Utkarsh's "prompting field guide" so scoring and steering are
consistent every time.

When scoring an existing prompt, the output is always four parts, in this order: **(1) a per-block
scorecard, (2) the mode & tier call, (3) the two or three real gaps in plain language, (4) an improved
rewrite.** Never skip the rewrite — the point of scoring is to fix, not just to judge.

---

## The Six Building Blocks

| Block | What it fixes | Grade it on |
|---|---|---|
| **Role** | Whose lens the answer takes — vocabulary, priorities, default assumptions. | Is a role set? Does it *fit* the task (a marketing lead for a campaign, not a PM)? |
| **Task** | The specific action or deliverable. Begins with a verb. | Is there a clear verb and a defined output? |
| **Context** | The situation the task sits in — audience, background, constraints, source material. | Is it present *and complete*, or present but too narrow? |
| **Requirements** | Format, length, tone, structure, required elements. | Are format AND length AND the required elements pinned down? |
| **Boundaries** | What *not* to do — limits on data, scope, or invention. | Are limits stated? Do they fence off a real failure mode? |
| **Reasoning** | A request to show the logic, or the rationale behind the ask. | Is the model asked to expose its logic / tag its work? |

---

## How to Grade Each Block

Assign one of three statuses. **The Partial-vs-Missing distinction is the most important judgment call** —
a block can be present but incomplete, and that is *not* the same as absent.

- **Success** — present and sufficient. The block does its job; the model won't have to guess here.
- **Partial** — present but underspecified or too narrow. *Examples:* a role is set but is an awkward fit;
  context names the audience but omits a dimension central to the task; requirements give a count but no
  format. Partial means "you started this block but left the model room to drift."
- **Missing** — absent entirely. The model will supply its own default, which is where fabrication,
  bias, and off-target output come from.

Reasoning and Boundaries are **Optional** for Basic/Focused tiers — mark them "Optional (not needed at
this tier)" rather than Missing when the task genuinely doesn't require them. They become required for
Comprehensive prompts.

### Grading heuristics
- **Role fit, not just presence.** "As a product manager" on a social-media caption task → Partial, because
  a marketing/social lead is the natural fit. Presence alone isn't Success.
- **Context completeness.** If the task implies a scope (all four brands, the full holiday range, the whole
  dataset) and the context covers only part of it → Partial, not Success. Name what's missing.
- **Requirements depth.** A count ("15 questions") without a format (answer key? layout? question types?)
  → Partial. Success needs format + elements + structure.
- **Boundary quality.** A vague "stay on strategy" is weaker than a specific fence ("don't recommend budget
  reallocations that reduce marketing spend"). Note when a boundary is sharp and when it's soft.

---

## Classify the Tier

After grading, name the tier so the person knows whether the prompt matches the stakes of the task.

- **Basic** = Task + Context. For straightforward, single-purpose tasks (a translation, a simple summary).
- **Focused** = + Role + Requirements. For work needing a professional lens and a set format (a competitive
  analysis, brand-specific materials, a templated plan).
- **Comprehensive** = all six blocks. For high-stakes judgment, output others will see, or competing
  priorities (sensitive decisions, leadership recommendations, cross-functional initiatives).

State it as: *"Reads as a [tier] prompt — core blocks present: N/4."* Then say whether that tier matches
the task, or whether the stakes call for moving up a tier.

**Key line to reinforce:** the tier measures how much the outcome depends on structure the person supplies,
not how important or effortful the task is. When a decision or an audience sits downstream, it's almost
always Comprehensive territory.

---

## Nudge Toward the Right Persona (Working Mode)

Scoring the blocks fixes *how* a prompt is written. Choosing the persona fixes *which mode of the model*
the task should call. Always name the right persona alongside the tier — a well-structured prompt aimed at
the wrong mode still underperforms. The rule of thumb: **match the muscle to the mission** — manual-muscle
tasks go to the Assistant, mental-muscle tasks go to the Strategist, and the Creator sits in between.

| Persona | Verb | Reach for it when | Don't, when |
|---|---|---|---|
| **The Assistant** | structure · sort · reshape | The task is mechanical and rule-based: reformatting, categorizing, extracting, de-duplicating, converting between formats. | The task needs judgment about what the data *means*, or a decision about what to do next. |
| **The Creator** | draft · produce · generate | You need a solid first draft to refine — a doc, deck, email, or outline that would otherwise start from a blank page. | You need it to invent facts or figures to fill the draft. Ask for `[DATA]` placeholders and supply the numbers yourself. |
| **The Strategist** | challenge · stress-test · interrogate | You're about to commit — to a decision, design, or pitch — and want blind spots and weak assumptions surfaced first. It can role-play skeptical stakeholders. | You'll take its confident claims as fact. This is the **highest-risk mode for fabrication** — verify anything it asserts. |

### How to infer the persona from the task
Read the person's *verb and intent*, not just their prompt:
- Reshaping, tidying, tagging, converting, summarizing raw material → **Assistant.**
- "Draft / write / generate / give me a first version of…" → **Creator.**
- "Is this a good idea / what am I missing / poke holes / evaluate / should I…" → **Strategist.**
- Producing a **test, worksheet, or article from source material** (e.g. Utkarsh's common tasks) → usually
  **Creator** for the drafting, but pair it with the fabrication guardrails below, since it's generating
  content that must stay true to the source.

Name the persona explicitly and say *why*, e.g. *"This is Creator work — you want a first draft to refine —
so keep the Strategist's 'challenge my assumptions' language out of it; that's a different mode."* When a
task spans modes, sequence them: *"Use the Creator to draft it, then the Strategist to pressure-test it
before it goes to leadership."*

---

## Start From the Goal, Even Without a Prompt

This skill nudges proactively. If the person describes a **goal or task but hasn't written a prompt yet**
("I need to make a fraction test", "help me write a launch announcement", "I want to decide between two
campaign directions"), don't wait for a prompt to score — steer them first:

1. **Name the persona** the task calls for (Assistant / Creator / Strategist) and why.
2. **Name the tier** the stakes call for (Basic / Focused / Comprehensive).
3. **List the blocks they'll need** for that tier, as a short checklist tailored to their task.
4. **Draft the prompt for them** using that structure, with `[bracketed placeholders]` for the details only
   they know — then run the normal scorecard on your own draft so they see it modeled.

The move is from *"could AI help me?"* to *"AI can do this specific task, in this mode, with these blocks."*
Turn a vague goal into a right-sized, right-mode prompt rather than asking the person to produce one cold.

---

## Always Check the Attachment / Fabrication Risk

This is a required check, not optional. Whenever a prompt refers to source material — "the doc I shared",
"the attached file", "the images", "our data", "the report" — verify whether that material is **actually
present in the conversation**.

- **If it is attached:** good. Still recommend the person *name the key concepts / contents* in the prompt
  as a backstop, so intent survives even if an attachment reads poorly.
- **If it is NOT attached:** flag it prominently. The model cannot pull from a file that isn't there; it
  will invent plausible-but-wrong content instead. This is the single most common way a well-structured
  prompt still fails. Tell the person to attach or paste the material before sending.

Frame it the way the field guide does: *a model fills whatever you leave blank, in the same confident voice
it uses when it's right.* A referenced-but-absent document is a blank.

---

## The Three Prevention Blocks (for fabrication-prone tasks)

When the task involves facts, figures, quotes, or source material, check specifically that these three
blocks are pulling their weight — they map onto the anti-fabrication defense:

1. **Context** — supply the real information and say "use only this." A task built on numbers the person
   provided can't fabricate those numbers; it can only do arithmetic on them.
2. **Boundaries** — forbid invention explicitly and give permission to say "I don't know" / "this source
   is unclear, which one did you mean?"
3. **Reasoning** — ask for traceable logic or per-item tagging so gaps become visible instead of hiding
   inside a confident paragraph.

And remind the person of the after-step: **verify before you ship** — cross-check anything consequential
against the source or a second tool; the final call is theirs.

---

## Output Format

**If the person gave a goal but no prompt yet,** use the "Start From the Goal" flow: name the persona, name
the tier, list the blocks they'll need, then draft a prompt for them and score your own draft. Otherwise,
when scoring an existing prompt, respond in exactly this structure:

### 1. Scorecard
One line per block: **Block → Status.** followed by a one-sentence reason. Cover all six. Use the real
status words (Success / Partial / Missing / Optional).

### 2. Mode & tier
One or two lines: name the **persona** the task calls for (Assistant / Creator / Strategist) and why, and
state the **tier** it reads as ("Reads as a [tier] prompt — core blocks present: N/4") plus whether that
matches the stakes or should move up. If the prompt targets the wrong mode, say so here.

### 3. The real gaps
Two or three short paragraphs naming what actually needs fixing — lead with the attachment check if any
source material is referenced, then the highest-impact gaps. Plain language, no bullet-point padding.

### 4. Improved prompt
A full rewrite as a blockquote, with each block labeled in bold (**Role.** **Task.** **Context.** etc.).
Fill real gaps with sensible defaults; where the person must supply something only they know (specific
concepts, actual numbers, which file), use a clearly-marked `[bracketed placeholder]` and point it out.

Close with one targeted question or a "check this before sending" note — most often the attachment check,
or an offer to run the improved prompt if the material is present.

---

## Tone

Direct and constructive, matching Utkarsh's preference for honest feedback. Praise the blocks that are
genuinely strong (don't flatter weak ones), name the gaps plainly, and always leave him with a better
prompt than he started with. Never soften a Missing into a Partial to be kind — the accuracy of the
grade is the whole value.
