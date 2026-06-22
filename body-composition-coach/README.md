# Body-Composition Coach (Agent Skill)

A goal-driven, evidence-based coaching skill. The user sets a body or health goal;
the skill tells them — honestly — whether and **how** it can be achieved, the
**risks**, and the physiologically **correct path**, then builds a personalized
home workout, a daily nutrition plan (ration), and a daily schedule to follow that
path. Every health claim is grounded only in real, citable sources (WHO and
national health authorities first), and the skill never invents facts or links.

This is an Anthropic **Agent Skill** — a folder of instructions Claude loads when
relevant. It does not run code or store data itself; it shapes how Claude responds.

---

## What it does

- **Appraises your goal first.** State a goal ("lose 8 kg", "reach 15% body fat",
  "build muscle", "get healthier") and the skill returns the goal restated with a
  realistic timeframe, *how* it's achieved, the *risks*, and the *body-correct
  path* — reframing unsafe or unrealistic goals toward a safe version instead of
  enabling them.
- **Covers fat loss, muscle gain, and recomposition** — not just scale weight. It
  factors in body fat %, desired muscle/lean mass, and overall health.
- **Assesses healthy targets from your body** — healthy weight range from height
  (BMI 18.5–24.9), with BMI's limitation for muscular people flagged, and
  waist-to-height ratio (< 0.5) as the adiposity metric that holds up at high
  muscle mass.
- **Builds a personalized plan** — home workout (equipment-aware, bodyweight if
  none), nutrition/ration (budget-, access-, preference-, and allergy-aware), and a
  daily schedule fitted to your time.
- **Optimizes for health ("heal the body")** — folds in sleep, activity, alcohol,
  stress, and disease-risk reduction.
- **Screens for safety first** and refers you to a clinician when a plan would be
  unsafe.
- **Speaks your language** — detects the language of your message and answers in it.
- **Cites real science, tiered honestly** — WHO / national health ministries
  (Tier 1) primary; clearly-labeled peer-reviewed consensus (Tier 2, e.g. ISSN)
  only where Tier 1 is silent, always marked as *not* a WHO/ministry source. Never
  fabricates URLs, studies, or numbers.

## How it works (flow)

1. **Safety screening** — checks red flags (pregnancy, heart conditions, diabetes
   on medication, recent surgery/injury, eating-disorder/body-image signals,
   under-18, etc.). If present → referral, not a plan.
2. **Goal appraisal** — how / risks / the body-correct path (the core coaching
   step). Works with minimal info.
3. **Intake** — collects what's needed for the concrete plan.
4. **Plan** — healthy-target assessment + training + nutrition + schedule +
   overall-health + sources.
5. **Sourcing policy** — Tier-1/Tier-2 citations, currency confirmed via web search
   when available.

## How to use it

Once the skill is installed/available, just talk to Claude naturally. Trigger
examples:

- "Help me lose fat and build muscle at home."
- "I want to get to 15% body fat — what's the right way, and is it safe?"
- "Make me a home workout and eating plan to slim down. I have dumbbells."
- "I want to get healthier — where do I start?"

**What it will ask you** (only what's missing): your goal; age / sex / height /
weight (and optionally body fat % and target muscle); days per week and minutes you
have; food budget, access, and cooking ability; injuries or conditions; food
likes/dislikes and allergies; and home equipment.

**What you get back:** first an honest appraisal of your goal (how, risks, correct
path); then — if you want it — a full plan (training, ration, schedule,
overall-health) with sources tagged Tier 1 / Tier 2.

### Environment notes

- **API-first.** The skill collects everything it needs in the conversation; it
  doesn't store personal data between sessions.
- **Web search is optional but recommended.** With search, the skill confirms the
  current edition of guidelines (these change — e.g. the US Dietary Guidelines moved
  to the 2025–2030 edition) and can pull in your national health ministry. Without
  search, it relies on the bundled verified sources and says so rather than
  inventing specifics.

## Files

```
body-composition-coach/
├── SKILL.md                 # the instructions Claude follows (name + description + workflow)
├── README.md                # this file
└── references/
    ├── safety.md            # red-flag protocol, ED/body-image handling, safe rates
    └── sources.md           # Tier-1/Tier-2 verified sources + search protocol
```

## Installation

Install in an environment that supports Agent Skills (e.g. Claude Code, or other
Claude products with skills enabled) by making the `body-composition-coach/` folder
available as a skill. To package it for distribution, run the skill-creator
packaging step to produce a `.skill` file you can install.

## Evidence & sourcing policy

The skill's value depends on never inventing sources. It uses two clearly-separated
tiers:

- **Tier 1 — WHO and national health ministries/agencies** (primary): healthy
  weight, physical activity, diet, and adiposity (waist-to-height ratio).
- **Tier 2 — peer-reviewed scientific consensus** (only where Tier 1 is silent,
  e.g. muscle-building protein targets): always labeled as *not* a WHO/ministry
  source.

Bundled sources are stable landing pages; specific numbers and editions should be
confirmed against the live source via search, because guidelines change.

## Safety & limitations

- **Not medical advice** — general educational information based on public-health
  and scientific guidance. Consult a doctor before starting.
- **Refers out when needed** — pregnancy, cardiovascular disease, diabetes on
  medication, recent surgery/injury, under-18, underweight, or symptoms on light
  activity → clinician referral, not a plan.
- **Handles eating-disorder and body-image signals carefully** — switches to
  supportive, non-numeric guidance and signposts to qualified help; won't produce
  crash diets or extreme body-fat targets.

## Configuration & open choices

- **Sourcing mode:** *Two-tier-transparent* (default — uses Tier 2 for muscle
  specifics, clearly labeled) vs *Strict* (Tier 1 / WHO-ministry only — muscle
  specifics given as general direction + referral).
- **Language:** automatic (mirrors the user).
- **National source:** the skill finds and prefers the user's national health
  ministry via search when the country is known.

## Maintenance

- **Sources go stale** — periodically re-verify the links and figures in
  `references/sources.md`; the skill prefers confirming the current edition via
  search at runtime.
- **Testing** — validate behavior with eval prompts (safety-gate, recomposition,
  unsafe goal, language mirroring) before shipping changes.
