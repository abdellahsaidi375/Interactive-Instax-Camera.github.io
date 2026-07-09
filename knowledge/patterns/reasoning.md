# Reasoning Pattern

## Summary

Reasoning patterns govern how models internally process problems before generating responses. From brief chain-of-thought to elaborate thinking tags, reasoning instructions shape the model's problem-solving approach, self-verification, and output quality.

## The Reasoning Spectrum

```
Simple Q&A ──► Brief CoT ──► Structured Thinking ──► Extended Reasoning
    │              │                │                       │
  Direct        2-3 step         Thinking tags           Full plan
  response      mental steps     with sections            + execution
```

## Observed Reasoning Implementations

### 1. Implicit Reasoning (All Providers, Baseline)

Every model is expected to reason internally, even without explicit instructions. The baseline pattern is:

```
Think step by step before answering complex questions.
Break down problems into manageable parts.
Verify your reasoning before responding.
```

This pattern is present in some form across ALL observed prompts.

### 2. Anthropic: Nuanced Analysis

**Claude Opus 4.6 Core:**
```
When faced with complex questions, think carefully and systematically.
Consider multiple perspectives before forming a conclusion.
Acknowledge uncertainty when appropriate.
Distinguish between well-established facts and interpretations.
Be precise about what you know and what you're uncertain about.
```

**Emphasis on:**
- Nuance and intellectual honesty
- Multiple perspective consideration
- Uncertainty acknowledgment
- Precision over comprehensiveness
- Depth over superficiality

### 3. OpenAI GPT-5 Thinking Mode

**Thinking Tags (explicit `thinking` blocks):**

GPT-5 thinking mode introduces explicit reasoning blocks:

```
<thinking>
[Internal reasoning goes here — not visible to the user]
</thinking>
```

The thinking block pattern:
1. **Analysis**: Break down the problem
2. **Approach**: Determine the method
3. **Verification**: Check assumptions
4. **Plan**: Outline the response structure
5. **Execute**: Generate the response

**Instructions for Thinking:**
```
Use the thinking block for:
- Complex multi-step problems
- Mathematical or logical reasoning
- Strategic planning
- Safety-relevant decisions
- Information synthesis from multiple sources

Do NOT use the thinking block for:
- Simple factual recall
- Greetings or social chat
- Short, straightforward answers
```

### 4. OpenAI o3 / o4-mini Reasoning Variants

These models have specialized reasoning chains:

**o3 Reasoning:**
```
Extended chain-of-thought reasoning for complex problems.
Break down problems into sub-problems.
Verify each step before proceeding.
Consider alternative approaches.
Self-correct when you detect errors.
Use mathematical notation for precise reasoning.
```

**o4-mini Reasoning (Compact):**
```
Efficient reasoning for simpler problems.
Focus on the core logic.
Skip elaboration when not needed.
Prioritize accuracy over thoroughness.
```

### 5. Anthropic Claude Opus Thinking Variants

Claude Opus thinking prompts include extended thinking mode:

```
<thinking>
You are in extended thinking mode.
Analyze the problem thoroughly.
Consider edge cases and alternatives.
Develop a structured approach.
Verify conclusions before output.
</thinking>
```

The thinking tag pattern is structurally similar to OpenAI's but with Anthropic's characteristic emphasis on nuance and multiple perspectives.

### 6. Kimi K2 Thinking (Moonshot AI)

The Kimi K2 thinking prompt observed in the corpus shows:

```
<thinking>
You are Kimi K2, a thinking model.
Approach problems methodically.
Use the following framework:
1. Understand: What is being asked?
2. Analyze: What information is available?
3. Reason: What logic applies?
4. Verify: Does the conclusion hold?
5. Respond: Formulate the answer.
</thinking>
```

### 7. Coding Agent Reasoning

**Claude Code:**
```
Before making changes:
1. Understand the request
2. Explore the relevant codebase
3. Plan the approach
4. Implement changes
5. Verify the result

When debugging:
1. Reproduce the issue
2. Isolate the cause
3. Consider multiple hypotheses
4. Test each hypothesis
5. Apply the fix
6. Verify resolution
```

**Cursor/Windsurf:**
```
Analyze the code context before suggesting changes.
Consider the impact of changes on other parts of the codebase.
Think about edge cases and error handling.
```

**Codex (GPT-5.5):**
```
Plan before coding:
1. Understand requirements
2. Design the solution
3. Consider tradeoffs
4. Implement
5. Test and verify

For each code change, consider:
- Does this solve the problem?
- Does this break anything?
- Is this maintainable?
- Is this performant?
```

## Reasoning Structures

### Sequential Reasoning (Most Common)

```
Step 1 ──► Step 2 ──► Step 3 ──► Step 4 ──► Conclusion
```

Used for: Math problems, multi-step instructions, debugging

### Branching Reasoning

```
              ┌─► Consideration A
Problem ──► Analyze ──► Consideration B ──► Synthesis ──► Conclusion
              └─► Consideration C
```

Used for: Strategic decisions, trade-off analysis, complex evaluation

### Recursive Reasoning

```
Problem ──► Solve ──► Verify ──► (If fails) ──► Re-analyze ──► Solve
                                          └─► Confirm ──► Done
```

Used for: Code verification, mathematical proofs, safety decisions

### Abductive Reasoning

```
Observation ──► Hypotheses ──► Evidence ──► Best explanation
```

Used for: Debugging, root cause analysis, diagnosing issues

## Self-Verification Patterns

### Universal "Check Your Work" Instructions

All prompts include some form of self-verification:

**General:**
```
Before responding, check:
- Does this answer the user's question?
- Is the information accurate?
- Is the response safe and appropriate?
- Does it follow formatting guidelines?
```

**Coding Agent:**
```
After making changes, verify:
- Does the code compile/run?
- Are all edge cases handled?
- Does it follow project conventions?
- Did you introduce any new issues?
```

**Safety-Critical:**
```
Before responding to potentially harmful requests:
1. Does this violate any safety category?
2. Could this be used to cause harm?
3. Is there a legitimate interpretation?
4. If uncertain, err on the side of caution.
```

## Reasoning Trigger Conditions

Models are instructed when reasoning is necessary versus when direct responses suffice:

| Condition | Response Mode |
|-----------|---------------|
| Simple factual query | Direct answer |
| Complex question | Structured reasoning |
| Multi-step problem | Sequential reasoning |
| Safety decision | Full analysis |
| Coding task | Plan → Implement → Verify |
| Mathematical problem | Step-by-step verification |
| Ambiguous request | Clarify before proceeding |
| Error scenario | Diagnose → Fix → Verify |

## Key Insights

1. **Thinking tags are the dominant explicit reasoning mechanism** across both Anthropic and OpenAI. Both use `<thinking>` blocks to separate internal reasoning from output.

2. **Coding agents follow a Plan → Execute → Verify cycle** rather than open-ended reasoning. Their reasoning is action-oriented, not exploratory.

3. **Self-verification is required, not optional.** Every prompt includes instructions to check work, verify safety, or confirm correctness before responding.

4. **Reasoning depth correlates with task complexity.** Instructions explicitly calibrate reasoning depth: simple tasks get direct answers, complex tasks get full reasoning chains.

5. **Safety reasoning is a separate track.** Even in models without explicit thinking tags, safety decisions receive special reasoning consideration — the model is instructed to think before refusing.

6. **Multi-perspective reasoning is Anthropic-specific.** Only Anthropic prompts emphasize considering multiple viewpoints before forming conclusions. Other providers favor linear correctness.
