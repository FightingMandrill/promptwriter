# Comprehensive Prompt Engineering Guide
## For LLMs Generating Detailed Prompts from High-Level Requirements

---

## Table of Contents
1. [Introduction & Philosophy](#introduction--philosophy)
2. [The RICE-FACT-ANCHOR Framework](#the-rice-fact-anchor-framework)
3. [Core Principles (Non-Contradictory Aspects)](#core-principles-non-contradictory-aspects)
4. [Decision Trees & Rules](#decision-trees--rules)
5. [Contradiction Resolutions](#contradiction-resolutions)
6. [Prompt Generation Workflow](#prompt-generation-workflow)
7. [Quality Checklist](#quality-checklist)

---

## Introduction & Philosophy

### What Makes a Good Prompt?

A good prompt:
- ✅ **Is explicit** — Don't make the LLM guess what to do
- ✅ **Uses natural prose** — Not just headers, but detailed explanations
- ✅ **Prevents attention degradation** — Uses mid-anchors for long context
- ✅ **Separates competing workflows** — Uses chain-of-thought when needed
- ✅ **Is readable** — Uses Markdown by default
- ✅ **Is token-efficient** — Avoids unnecessary complexity

### The Underlying Insight

LLM performance depends on:
1. **Task clarity** — Model must know exactly what to do
2. **Attention distribution** — Model's focus doesn't degrade in the middle
3. **Computational workflow separation** — Multiple task types need multiple steps
4. **Context ordering** — Task clarity depends on context size

These aren't contradictory principles. They work together.

---

## The RICE-FACT-ANCHOR Framework

### Core Structure

```
# Role
[Natural language defining persona/scope of knowledge]

# Instruction/Task  
[Natural language describing the work - include explicit action verb(s)]

# Context
[Relevant background information, NOT stories or dumps]
[Why this matters, what to focus on]

# Examples
[2-4 concrete input/output examples showing format and approach]
[Only repeat in early position if task is very complex]

# Format
[Natural language describing output structure, length expectations]
[What format, what structure, what length]

# Action/Goal
[What the model should "think around" - the goal state]
[Why this matters, what success looks like]

# Tone
[Style and voice expectations]
```

### Ordering Rules (Critical)

**For short context (<500 tokens):**
- Use RICE-FACT order as-is

**For medium context (500-2000 tokens):**
```
Role → Task → Examples → Format → Action/Goal → Context → Tone
```
(Move Examples/Format before long Context to avoid middle degradation)

**For long context (>2000 tokens):**
```
Role → Task → Format → Action/Goal → [Examples optional]

=== CONTEXT CHUNK 1 ===
[First part of content/documents]

=== MID ANCHOR ===
Remember: [Role summary] | [Task summary] | [Goal summary]

=== CONTEXT CHUNK 2 ===
[Second part of content/documents]

=== MID ANCHOR (if needed) ===
[Repeat mid-anchor]

=== END ANCHOR ===
Final Checklist:
1. Did you follow the Role?
2. Did you complete the Task?
3. Is your output in Format?
4. Did you achieve the Goal?

=== YOUR OUTPUT ===
```

**For ambiguous tasks (task unclear without context):**
```
Role → Task (abstract) → Context → Task (clarified) → Examples → Format → Action/Goal
```

---

## Core Principles

### Structure & Clarity
- **Use natural language prose, not just headers** — Write like talking to a human. Full sentences, not bullet points. Explain constraints within prose naturally.
- **Use clearly labeled sections** — Headers (Role, Task, Context, etc.) help models parse intent and create cognitive boundaries
- **Every task needs an explicit action verb** — Verbs signal computational mode (Analyze ≠ Summarize). Models must know what mode to activate.

### Specificity
- **Be specific, not vague** — Don't hide behind ambiguous language. "Important findings" should mean something concrete. Explain what you're looking for.
- **Keep instructions concise** — Say what's needed without bloat. Each instruction should be clear on a single read.
- **Remove unnecessary content** — Strip emotional filler ("Please make this really great!") and meta-notes ("As we discussed..."). Keep only execution-relevant information.

### Attention Management
- **Use mid-anchors for long context** — When context exceeds ~1000 tokens, reset attention with reminders every ~1500 tokens. Prevents models from "getting lost in the middle."
- **Strategic constraint specification** — Hard constraints use MUST/NEVER (will break the system). Soft constraints use TRY TO/AIM FOR (acceptable to skip for quality).
- **Use checklist at the end** — Final verification step: Did you follow the role? Complete the task? Match the format? Achieve the goal?

### Task Decomposition
- **Break complex tasks into steps** — When a task involves multiple computational workflows (e.g., analyze → compare → recommend), use chain-of-thought structure. Each step isolates one workflow type.
- **Use numbered lists for multi-step tasks** — Makes priority and order explicit. Eliminates ambiguity about task sequence.

### Format & Efficiency
- **Default to Markdown** — Most readable, most token-efficient, in-distribution for models. Override to JSON/XML only if explicitly required by the system.
- **Examples clarify format and approach** — Use 2-4 concrete examples showing input/output. Examples teach the model what good looks like without requiring lengthy explanation.

---

## Decision Trees & Rules

### Decision 1: Context Size & Ordering

```
Estimate total context tokens (including examples, background, documents):

< 500 tokens?
  → Use RICE-FACT default order
  → No anchors needed

500-2000 tokens?
  → Move Examples/Format before Context
  → Task → Examples → Format → Action/Goal → Context
  → No anchors needed (usually)

> 2000 tokens?
  → Fragment Context with mid-anchors
  → Add anchor every ~1500 tokens of content
  → Role → Task → Format → Action/Goal → [Content with mid-anchors]

Ambiguous task?
  → Context clarifies what "good" means
  → Order: Task (abstract) → Context → Task (clarified)
```

### Decision 2: Chain-of-Thought Necessity

```
Count distinct computational workflows required:

1 workflow (e.g., pure analysis, pure summarization)?
  → Single-pass is fine
  → Use mid-anchors if context is long

> 1 workflow (e.g., analysis + style transformation, retrieval + reasoning)?
  → Use multi-step chain-of-thought
  → Each step isolates one workflow type
  → Examples: Analyze → Compare → Recommend (3 workflows)

If using CoT:
  - Each step can be short (avoids middle degradation)
  - Use mid-anchors within steps only if that step has long context
  - Don't repeat examples at each step (only repeat Role/Task/Goal)
```

### Decision 3: Constraint Specification

```
For each constraint, determine risk level:

HIGH RISK if violated (breaks systems/requirements):
  → "Output MUST be valid JSON"
  → "MUST include these 5 topics"
  → Use: MUST, NEVER, ABSOLUTELY, REQUIRED

MEDIUM RISK if violated (hurts but doesn't break):
  → "Output should include both analysis and recommendation"
  → "Must maintain professional tone"
  → Use: ENSURE, INCLUDE, MAINTAIN, MUST HAVE

LOW RISK if violated (preference, quality can override):
  → "Try to keep under 150 words"
  → "Aim for conversational tone"
  → Use: TRY TO, AIM FOR, PREFER, SHOULD

If unclear:
  → Ask user: "What breaks if this is violated?"
  → Ask: "Is this required or preferred?"
  → Ask: "How critical is this relative to output quality?"
```

### Decision 4: Task Verb Specification

```
Every task section MUST include at least one action verb.
The verb signals the computational mode the model should activate.

Task categories (examples of equivalent verbs):

Summarization:     Summarize, Condense, Abbreviate, Extract main points
Extraction:        Extract, Identify, Pull out, Find, Locate
Analysis:          Analyze, Assess, Evaluate, Examine, Review
Generation:        Write, Create, Generate, Draft, Compose
Comparison:        Compare, Contrast, Differentiate, Distinguish
Classification:    Classify, Categorize, Label, Tag, Sort
Transformation:    Rewrite, Rephrase, Paraphrase, Convert, Transform
Ranking:           Rank, Order, Prioritize, Sort by importance
Evaluation:        Judge, Rate, Score, Assess, Evaluate

Rules:
- Use ANY verb from the right category (don't overthink exact wording)
- For multiple tasks, use multiple verbs (numbered or clear structure)
- Verb should come early: [Verb] + [Object] + [Constraints]
- Task verbs act as mode switches (Summarize ≠ Analyze)
```

### Decision 5: Example Count & Placement

```
Default: 2-4 examples (few-shot beats one-shot)

By task complexity:

Simple task (obvious output format):
  → 0-1 examples
  → Example: "Classify as positive/negative"

Medium task (some nuance to format or approach):
  → 2-4 examples
  → Example: "Analyze sentiment and extract themes"

Complex task (many edge cases, nuanced judgments):
  → 5-7 examples
  → Example: "Analyze sentiment across multiple dimensions"

Never bloat:
  → Don't use 10+ examples (wastes tokens, adds noise)
  → Quality of examples > quantity

Placement:

Default (RICE-FACT):
  → Examples go after Context

Exception (for very complex goals):
  → 1-2 key examples right after Task
  → Full example set in RICE-FACT position
  
Multi-step/anchors:
  → Examples DO NOT repeat at mid-anchors
  → Only Role/Task/Context/Goal get repeated
```

### Decision 6: Delimiter Choice

```
Default: Markdown

Use Markdown for:
- Human-readable output
- No explicit format requirement
- Prompts, instructions, analysis
- Content where readability matters most

Override to JSON if:
- User explicitly requests JSON
- Output is a data structure (API payload, database record)
- Machine parsing is the primary consumer

Override to XML if:
- User explicitly requests XML
- Output requires hierarchical tagging with attributes
- Legacy system requires XML input

All other cases:
- Markdown headers, lists, tables, code blocks
```



## Prompt Generation Workflow

### Step 1: Clarifying Questions (Required Information)

**CRITICAL - Must have answers before proceeding:**

1. **What is the primary goal?**
   - What should the LLM actually output/do?
   - What is success?

2. **What is the context window size?**
   - Small: <500 tokens of context
   - Medium: 500-2000 tokens
   - Large: >2000 tokens

3. **What information/files will be provided?**
   - Documents, data, background information
   - Helps estimate context size

**NICE TO HAVE - Improves prompt quality:**

4. **What category of prompt is this?**
   - Long context (requires mid-anchors)
   - Chain-of-thought (requires multi-step)
   - Structured reasoning (requires explicit workflows)
   - Agentic (requires loops/iteration)

5. **Are there explicit format requirements?**
   - JSON API format?
   - Markdown?
   - Specific structure?

6. **What are the hard constraints?**
   - Non-negotiable requirements
   - System compatibility requirements
   - Legal/compliance requirements

### Step 2: Determine Prompt Structure

**Based on context size:**
- If small: Use RICE-FACT default
- If medium: Reorder with Examples/Format before Context
- If large: Plan for mid-anchors

**Based on task complexity:**
- If single workflow: Single-pass is fine
- If multiple workflows: Plan for chain-of-thought steps

**Based on constraint clarity:**
- If all clear: Use categorized constraints (Hard/Medium/Soft)
- If unclear: Build clarifying questions into the prompt

### Step 3: Write Each Section

**Section 1: Role**
- Natural language defining persona/scope
- Limit scope of knowledge (what the model should focus on)
- Example: "You are a marketing analyst specializing in B2B SaaS"

**Section 2: Task**
- Natural language starting with explicit action verb
- Include primary objective and sub-tasks (if multiple)
- Embed constraints naturally in prose
- Example: "Analyze the quarterly updates to identify rate changes and tenure reductions..."

**Section 3: Context** (placement depends on size)
- Provide relevant background
- Don't tell stories, don't dump data
- Explain WHY this matters
- Example: "Context: You're analyzing home loan statements where rate drops affect remaining EMI duration..."

**Section 4: Examples**
- 2-4 concrete input/output examples
- Show format, approach, and edge cases
- Label as Example 1, Example 2, etc.

**Section 5: Format**
- Describe output structure, length, format
- Use Markdown by default unless explicit requirement
- Example: "Output as a Markdown table with columns: [Quarter], [Rate], [Remaining EMIs], [Months Saved]"

**Section 6: Action/Goal**
- State what model should "think around"
- What is success?
- Why does this matter?
- Example: "Goal: Calculate total months saved across all quarters to understand cumulative loan acceleration"

**Section 7: Tone**
- Style and voice expectations
- Example: "Be objective and analytical. Focus on data, not interpretation."

### Step 4: Add Mid-Anchors (If Context > 1000 tokens)

**Placement:** Every ~1500 tokens of content

**Content:**
```
=== MID ANCHOR ===
Remember your Role: [1 line role summary]
Your Task: [1 line task summary]
Key Goal: [1 line goal summary]
```

**At End:**
```
=== END ANCHOR ===
Final Checklist:
1. Did you understand the Role?
2. Did you complete the Task?
3. Is your output in the correct Format?
4. Did you achieve the Goal?
```

### Step 5: Quality Checklist

Before finalizing:

- [ ] Every task has an explicit action verb (Analyze, Extract, Calculate, etc.)
- [ ] No vague instructions (e.g., "important" needs to mean something specific)
- [ ] All hard constraints use MUST/NEVER language
- [ ] All soft constraints use TRY TO/AIM FOR language
- [ ] Examples are concrete and show edge cases
- [ ] Context is ordered correctly for its size
- [ ] No mid-anchors needed OR mid-anchors are placed every ~1500 tokens
- [ ] Tone is set explicitly
- [ ] Format is specified (preferably Markdown)
- [ ] Goal is clear (not buried, not vague)

---

## Prompt Generation Template

### Template: Single-Pass (Short Context, Single Workflow)

```markdown
# Role
[Natural language. Define scope. Keep it tight.]

# Task
[Verb] [object]. [Constraints]. [More specifics].
[What NOT to do.]

# Context
[Background that matters. Why this task.]
[Focus on X, ignore Y.]

# Examples
Example 1:
Input: [...]
Output: [...]

# Format
[Output structure using Markdown tables/lists/prose]
[Length expectations]

# Action/Goal
[What success looks like]
[Why this matters]

# Tone
[Style expectations]
```

### Template: Multi-Step (Multiple Workflows)

```markdown
# Role
[...]

# Task
Your goal is to [primary goal]. Break it into these steps:

1. [Task verb]: [First workflow]
2. [Task verb]: [Second workflow]  
3. [Task verb]: [Third workflow]

For each step:
- [Step-specific constraints]

# Context
[Background information]

# Examples
[Show example output for the full multi-step process]

# Format
Present each step clearly:
- Step 1 output: [format]
- Step 2 output: [format]
- Step 3 output: [format]

# Action/Goal
[What the full sequence achieves]

# Tone
[...]
```

### Template: Long Context with Mid-Anchors

```markdown
# Role
[...]

# Task
[...]

# Format
[...]

# Action/Goal
[...]

[CONTENT CHUNK 1 - approximately 1500 tokens of documents/context]

=== MID ANCHOR ===
Remember: 
- Role: [1 line]
- Task: [1 line]
- Goal: [1 line]

[CONTENT CHUNK 2 - approximately 1500 tokens]

=== MID ANCHOR ===
[Repeat if needed for very long context]

=== END ANCHOR ===
Final Checklist:
1. Role: Did you act as [role]?
2. Task: Did you [task verb]?
3. Format: Is output in [format]?
4. Goal: Did you achieve [goal]?

=== YOUR OUTPUT ===
```

---

## Prompt Quality Metrics

A good prompt should:

1. **Clarity (0-10)**
   - Can the LLM understand what to do without guessing?
   - Do task verbs make the mode clear?

2. **Completeness (0-10)**
   - Are all necessary details present?
   - Would a human understand this task?

3. **Token Efficiency (0-10)**
   - No unnecessary words or structure
   - No XML/JSON unless explicitly required
   - Markdown where possible

4. **Attention-Safe (0-10)**
   - No critical information in "lost in middle" position
   - Mid-anchors present if context > 1000 tokens
   - Examples placed strategically

5. **Constraint Clarity (0-10)**
   - Hard constraints clearly marked (MUST, NEVER)
   - Soft constraints marked (TRY TO, AIM FOR)
   - No ambiguous "should" statements

6. **Task Separation (0-10)**
   - Multiple workflows get multiple steps
   - Each step has clear verb and objective
   - No mixing of incompatible modes

---

## Examples: From High-Level Ask to Detailed Prompt

### Example 1: Loan Email Analysis (Your Use Case)

**High-level ask:**
"I need a prompt to analyze my quarterly emails - status update on my home loan. My loan rate drops and my total tenure (i.e. the # of remaining EMIs drops). I want to pull all quarterly status updates, see the remaining EMIs and calculate net of elapsed time how many months have been saved by rate drops"

**Questions to ask:**
- How many quarterly emails? (context size)
- What format are they in? (plain text, HTML, attachments?)
- What is success? (a spreadsheet? a summary? a visualization?)
- Are there hard constraints on output format?

**Generated prompt (assuming: 10 emails, plain text, wants a summary table):**

```markdown
# Role
You are a financial analyst specializing in home loan analysis.
Your knowledge scope: EMI calculations, interest rate mechanics, loan tenure.

# Task
Extract and analyze quarterly home loan status updates to calculate total months saved due to rate drops.

Your specific steps:
1. Extract from each email: [Quarter], [Interest Rate], [Remaining EMIs]
2. Calculate the theoretical EMI duration for each quarter
3. Compare consecutive quarters to identify rate changes and EMI reductions
4. Calculate months saved by each rate drop (difference in tenure)
5. Total across all quarters: How many months of loan tenure have been accelerated?

# Context
Home loan status updates typically include:
- Current interest rate (what you're paying annually)
- Remaining EMIs (how many payments left)
- Total outstanding principal
- Sometimes projected payoff date

A rate drop means:
- Your interest rate decreases
- Your EMIs might stay same (accelerating payoff) or decrease (more cash flow)
- Your remaining tenure shortens

You need to track how much total time has been saved across all drops.

# Examples
Example 1:
Q1 2023: Rate 6.5%, Remaining EMIs: 240 (20 years)
Q2 2023: Rate 6.2%, Remaining EMIs: 235 (19.6 years)
Analysis: Rate dropped 0.3%. Tenure reduced by 0.4 years = ~5 months saved

# Format
Output as a Markdown table:
| Quarter | Rate | Remaining EMIs | Months Saved | Cumulative Months |
|---------|------|-----------------|--------------|-------------------|
| Q1 2023 | 6.5% | 240 | 0 | 0 |
| Q2 2023 | 6.2% | 235 | ~5 | 5 |

Then add:
Summary: Total months saved across all quarters: [X months]

# Action/Goal
Calculate how much your loan has been accelerated due to rate drops.
This shows the benefit you've received from refinancing or rate reductions over time.

# Tone
Be precise and data-focused. Show your calculation methodology briefly.
```

### Example 2: Competitor Analysis (Multi-Step)

**High-level ask:**
"Write a prompt to analyze our top 3 competitors, understand their positioning, and recommend one strategic move we should make"

**Generated prompt:**

```markdown
# Role
You are a strategic business analyst.
Expertise: competitive positioning, market strategy, B2B SaaS dynamics.

# Task
Analyze our top 3 competitors and recommend one strategic action.

Break this into these steps:

1. Analyze: For each competitor, extract their core positioning, pricing model, target customer
2. Compare: How do we differ from each? Where do they beat us? Where do we beat them?
3. Synthesize: What is the single biggest competitive gap?
4. Recommend: What is ONE thing we should build/change/do to close that gap?

# Context
Competitive positioning in B2B SaaS depends on:
- Who they target (SMB vs. Enterprise, vertical focus, use case)
- What problems they solve best
- How they price (per-user, per-usage, enterprise deals)
- Where they have distribution advantage

A strategic recommendation should be:
- Specific (not "improve product")
- Defensible (based on actual competitor gaps, not guesses)
- Achievable (within 6-12 months)

# Examples
Example (for structure only):

Competitor 1: Acme Inc
- Positioning: "Easiest CRM for small teams"
- Pricing: $50/user/month
- Strength: UX simplicity, customer support
- Weakness: Limited customization, no API ecosystem

Comparison: We position on "powerful customization for enterprises"
- Gap: They own simplicity; we own power. No direct conflict.
- Opportunity: Their customers outgrow them; we could offer upgrade path.

Recommendation: Build easy migration path from Acme to our platform.
- Rationale: Their customers hit ceiling; we catch them before competitor acquires them.

# Format
For each competitor:
[Name]: [Positioning] | [Pricing] | [Strength] | [Weakness]

Comparison:
- Our advantage: [X]
- Their advantage: [Y]
- Gap: [Z]

Recommendation:
Action: [Specific thing to do]
Rationale: [Why this matters]
Timeline: [How long]

# Action/Goal
Identify the highest-leverage strategic move to strengthen our competitive position.

# Tone
Data-driven. Base recommendations on actual competitor strengths/weaknesses, not hunches.
```

---

## Summary: This Guide Synthesizes

✅ **Gemini:** Natural language prose, specificity, simplicity
✅ **GPT-4.1:** Clear structure, strategic repetition, chain-of-thought
✅ **Ruleset:** Context hygiene, load management, salience
✅ **Our Discussions:** All 8 contradiction resolutions

**Result:** A unified, actionable framework for generating prompts that work across model types and task complexities.

