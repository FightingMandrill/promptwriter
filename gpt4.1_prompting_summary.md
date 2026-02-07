# GPT-4.1 Prompting Guide Summary (from OpenAI Cookbook)

## Section 5: General Advice

### 1. **Prompt Structure**
Use clearly labeled sections to structure your prompts. Recommended headers:
- `# Role and Objective`
- `# Instructions`
- `## Sub-categories` (for more detailed rules)
- `# Reasoning Steps`
- `# Output Format`
- `# Examples`
- `## Example 1`
- `# Context`
- Final instruction block prompting the model to think step by step

### 2. **Delimiter Formats**
Different formats work better depending on the task:
- **Markdown**: Ideal for most prompts. Use headings (`#`), backticks for code, and bullet lists.
- **XML**: Best when you need start-end sections with optional metadata. Especially effective in long contexts.
- **JSON**: Strong in coding settings but verbose and harder to write manually.
- **Plain text with tags**: Formats like `ID: 1 | TITLE: ...` also perform well.

Avoid using noisy, redundant, or overly verbose delimiters. Stick to formats that clearly separate intent and sections.

### 3. **Instruction Placement (Long Context Specific)**
- For long contexts, **repeat instructions at the beginning and end** for best performance.
- If including only once, **place at the beginning**.

### 4. **Chain-of-Thought (CoT) Prompting**
- Prompting the model to reason step-by-step improves complex task outcomes.
- Basic CoT line:  
  _"Think carefully step-by-step before answering..."_
- Audit errors and refine CoT prompts by specifying steps more clearly.

### 5. **Instruction Tuning Best Practices**
- Include a top-level "Instructions" section with bullets.
- Add sub-sections for categories like tone, tools, format.
- Use examples to reinforce behavior.
- Avoid overly rigid instructions (e.g., "MUST use this tool") unless necessary.
- Avoid redundant, all-caps, or bribery phrasing unless results demand it.
- Make sure sample phrases don't make outputs repetitive — tell the model to vary them.

### 6. **Known Pitfalls**
- Models might hallucinate tool calls if told to “always” call them.
- Sample phrases might lead to repetitive responses.
- Without tone constraints, GPT-4.1 may be overly verbose or formal.



---

## Section 1: Agentic Workflows – General Advice

### Key Setup Reminders for Agent Prompts

When designing agent prompts for GPT-4.1, OpenAI recommends including three types of reminders:

1. **Persistence** – Clarify that the model should continue across multiple turns and not hand off prematurely.
   > _"You are an agent – please keep going until the user’s query is completely resolved."_

2. **Tool-Calling** – Explicitly encourage the model to use tools, reducing hallucinations.
   > _"If you are not sure about file content or codebase structure pertaining to the user’s request, call the appropriate tool instead of guessing."_

3. **Planning (Optional but Recommended)** – Instruct the model to reflect before acting.
   > _"You MUST plan extensively before each function call, and reflect on the output after each step."_

These dramatically increase model autonomy and initiative in complex workflows.

---

### Tool Design Guidance

- **Use the `tools` API field** to define tools, not raw prompt text with inline schemas.
- **Name tools clearly** and write concise but thorough descriptions.
- **Avoid bloating descriptions** with examples—use a dedicated `# Examples` section for that.
- When tool behavior is complex, **document edge-case usage** in your prompt or evals.

---

### Planning + Chain-of-Thought Integration

- GPT-4.1 can be induced to “think out loud” with planning prompts even though it's not inherently a reasoning model.
- In SWE-bench, adding explicit planning improved success rates by 4%.

---

### Best Practices Summary
| Practice            | Effect                                                              |
|---------------------|---------------------------------------------------------------------|
| Add persistence     | Prevents premature turn termination                                 |
| Encourage tool use  | Lowers hallucination, improves accuracy                            |
| Explicit planning   | Boosts complex task completion, mimics chain-of-thought             |
| Use API tool schema | Ensures in-distribution behavior and better tool interaction        |
| Tool descriptions   | Keep concise; offload verbose examples to `# Examples`              |


---

## Section 2: Long Context

### General Advice

- GPT-4.1 performs well with context windows up to 1M tokens.
- Ideal for tasks such as:
  - Multi-hop reasoning
  - Structured document parsing
  - Context re-ranking
  - Extracting relevant details from noisy context
- **Instruction placement matters**:
  - Best: repeat instructions at both beginning and end
  - Acceptable: only at the beginning
  - Worst: only at the end
- Context performance degrades as:
  - Retrieval complexity increases
  - Full-context understanding is needed (e.g., graph reasoning)
- Choose instruction detail level based on expected reasoning burden.
- Consider instructing the model on how to balance external (prompt) and internal (model) knowledge.

### Prompt Templates

```text
# Long Context Prompt
# Instructions
Use only the documents provided in the External Context to answer the User Query. If information is missing, indicate what’s missing.

# External Context
{external_context}

# User Query
{user_question}

# Chain-of-Thought
First, think carefully step by step about what documents are needed to answer the query...
```

---

## Section 3: Chain of Thought (CoT)

### General Advice

- GPT-4.1 is not a reasoning model by default, but can be induced to reason through prompting.
- Basic prompt pattern: _“Think step by step...”_ improves logical flow and correctness.
- CoT increases latency and cost (more output tokens), so use when necessary.
- Review failed CoT outputs:
  - Misunderstood user intent
  - Missed/relevant context
  - Poor step planning
- Codify successful CoT strategies into templates for reuse.

### Prompt Templates

```text
# Chain-of-Thought (General)
First, think step-by-step about what the user is asking. Identify any key documents or background knowledge required. Only then attempt a final answer.

# Reasoning Strategy
1. Query Analysis
2. Context Review
3. Structured Step-by-Step Answer
```

---

## Section 4: Instruction Following

### General Advice

- GPT-4.1 is more literal and precise with instructions than prior models.
- Reuse previous prompts cautiously; implicit expectations may no longer work.
- Write high-level rules up front under a section titled “Instructions”.
- Add sub-sections to detail rules (e.g. Sample Phrases, Output Format).
- Add examples to show correct behavior in context.
- Place conflicting instructions toward the end if you want them prioritized.
- Avoid excessive caps, incentives, or over-specific formats—use only when needed.
- Typical failure modes:
  - Over-adherence to tool rules (e.g., calling with null values)
  - Repetition of sample phrases
  - Overly verbose or formatted responses

### Prompt Templates

```text
# Instructions
- Greet users with: "Hi, you've reached [Company], how can I help you?"
- Always call a tool to answer factual questions. If unsure, ask the user for more information.
- Never discuss politics, religion, or controversial topics.

# Response Steps
1. Greet
2. Call Tool (if needed)
3. Reply with concise, helpful message
4. Ask if further help is needed

# Sample Phrases
- "Let me check that for you..."
- "Here’s what I found:"
```
