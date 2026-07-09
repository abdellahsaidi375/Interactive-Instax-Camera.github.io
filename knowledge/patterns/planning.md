# Planning Pattern

## Summary

Planning governs how models decompose complex tasks into manageable steps, sequence operations, manage dependencies, and track progress. While chatting agents use planning implicitly, coding agents have elaborate planning instructions as the core of their task execution model.

## The Planning Spectrum

```
Implicit Planning                 Explicit Planning               Formal Planning
(All chat models)                 (Coding agents)                 (Structured tools)
    │                                  │                               │
  Mental steps                     Declared plan                   JSON/structured plan
  No visible plan                  User-visible steps              Machine-parseable
  Re-plan on the fly               Iterative refinement            Strict execution
```

## Observed Planning Implementations

### 1. Anthropic Core Chat: Implicit Planning

**Claude Opus 4.6:**
```
When asked to perform complex tasks, approach them methodically.
Break down the task into manageable parts.
Consider each part carefully before proceeding.
```

Planning is **implicit** — the model plans internally but does not declare a plan to the user except in complex cases. The expectation is natural, conversational task execution.

### 2. OpenAI GPT-5: Situational Planning

**GPT-5 thinking:**
```
For multi-step tasks, outline your approach before executing.
Present the plan briefly, then execute each step.
If the plan needs adjustment, explain the change.
```

Planning is **situational** — declared only when tasks have clear steps. The model decides when planning is needed.

### 3. Claude Code: Structured Plan-Execute-Verify

**Claude Code has the most elaborate planning architecture:**

```
PLANNING PHASE:
When given a complex task:
1. Understand: Clarify what needs to be done
2. Explore: Read relevant files and understand current state
3. Plan: Outline the approach step by step
4. Confirm: Optionally present the plan to the user
5. Execute: Implement each step
6. Verify: Check that each step works
7. Iterate: Fix any issues discovered

PLAN STRUCTURE:
For each step, specify:
- What needs to be done
- Which files will be affected
- What tools will be used
- Dependencies on other steps
```

**Step Dependency Management:**
```
Sequential steps: Execute in order (Step B depends on Step A)
Parallel steps: Can execute simultaneously (no dependencies)
Conditional steps: Execute only if condition is met
Loop steps: Repeat until condition is satisfied
```

### 4. OpenAI Codex (GPT-5.5 Full): Plan-Driven Development

**Codex has explicit development phases:**

```
PHASE 1: UNDERSTAND
- Read the existing codebase
- Understand the requirements
- Identify relevant files

PHASE 2: DESIGN
- Determine the approach
- Consider trade-offs
- Plan the implementation

PHASE 3: IMPLEMENT
- Write or modify code
- Follow project conventions
- Add tests

PHASE 4: VERIFY
- Run tests
- Check for errors
- Verify requirements met

PHASE 5: REVIEW
- Review changes holistically
- Consider edge cases
- Refine if needed
```

### 5. Cursor / Windsurf: Integrated Code Planning

```
When helping with code:
1. Search the codebase for relevant context
2. Read current implementations
3. Understand patterns and conventions
4. Plan changes considering architecture
5. Implement with appropriate granularity
6. Verify through testing or review
```

Cursor and Windsurf emphasize **codebase-aware planning** — understanding the project structure before making changes.

### 6. Cline: Sequential Tool Planning

```
For any multi-step task:
1. Plan the sequence of tool calls
2. Execute one step at a time
3. Evaluate each result before proceeding
4. Adapt the plan based on intermediate results

Tools execute sequentially — each step depends on the previous result.
```

### 7. Replit: Environment-Aware Planning

```
Before making changes:
1. Understand the Replit environment
2. Read the current code
3. Plan changes considering environment constraints
4. Implement and test within the environment
5. Verify the application runs correctly
```

### 8. Jules (Google): Agentic Planning

```
Jules plans development tasks autonomously:
1. Analyze the repository structure
2. Identify files to modify
3. Create a detailed implementation plan
4. Execute changes with validation
5. Review and summarize changes
```

## Planning Dimensions

### Granularity Control

| Level | Description | Used When |
|-------|-------------|-----------|
| **Macro-plan** | High-level phases | Large features, multiple files |
| **Meso-plan** | Step groups | Single feature, few files |
| **Micro-plan** | Individual operations | Simple task, single file |
| **No plan** | Direct execution | Trivial operations |

### Planning Triggers

Planning is triggered by:
- **Complexity**: Multiple steps needed (coding agents)
- **Novelty**: First time performing this task
- **Risk**: Destructive or irreversible operations
- **Exploration**: Unfamiliar codebase
- **User request**: "Plan this out" or explicit planning instruction

### Plan Presentation

| Presentation | Usage | Providers |
|-------------|-------|-----------|
| **Implicit** | Internal planning, hidden from user | Chat models |
| **Brief summary** | 1-3 line overview of approach | GPT-5, Gemini |
| **Step list** | Numbered steps | Claude Code, Codex |
| **Structured plan** | Sectioned with dependencies | Claude Code (complex) |
| **Interactive plan** | Ask "Shall I proceed?" before executing | Claude Code, OpenCode |

### Plan Adaptation

All coding agents include instructions for adapting plans mid-execution:

```
When a step fails or produces unexpected results:
1. Analyze the result
2. Determine if the plan needs adjustment
3. Explain the change to the user
4. Continue with the modified plan

Do NOT blindly continue when results contradict expectations.
```

## Verification-Driven Planning

A key pattern across coding agents is **verification built into the plan**:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Step 1  │───►│  Verify  │───►│  Step 2  │───►│  Verify  │───►...
│  Implement│   │  Step 1  │    │  Implement│   │  Step 2  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
```

Each step is followed by a verification action (compile, test, review) before proceeding.

## Common Planning Anti-Patterns

1. **Premature execution**: Planning and implementing in one step without verification. Mitigated by explicit plan-review-execute phases.
2. **Over-planning**: Creating elaborate plans for trivial tasks (e.g., "editing one line"). Mitigated by granularity rules.
3. **Brittle plans**: Plans that fail when any step has unexpected results. Mitigated by adaptation instructions.
4. **Scope creep**: Expanding plan mid-execution. Mitigated by explicit scope boundaries.
5. **Missing dependencies**: Not checking prerequisites. Mitigated by dependency mapping steps.

## Key Insights

1. **Planning is the core differentiator between chat and coding agents.** Chat models plan implicitly (or not at all); coding agents have Plan → Execute → Verify as their fundamental operational loop.

2. **Verification is not optional.** Every coding agent prompt includes verification steps after every change. This is the single most consistent coding agent pattern.

3. **Plan adaptation is required.** Naive prompts say "follow the plan"; sophisticated prompts say "adapt the plan when results contradict expectations."

4. **Interactive planning** (asking "shall I proceed?") is the standard for potentially impactful operations. Autonomous execution is reserved for clearly safe, reversible actions.

5. **Plan visibility varies by provider.** Anthropic's Claude Code shows plans to users; OpenAI's Codex keeps plans internal but presents results. This reflects different philosophies about user agency vs. efficiency.
