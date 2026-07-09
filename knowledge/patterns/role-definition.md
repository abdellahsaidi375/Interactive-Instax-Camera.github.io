# Role Definition Pattern

## Summary

All major prompt systems begin with an explicit identity declaration establishing the model's name, creator, capabilities, and interaction stance. This pattern creates the foundational frame through which every subsequent instruction is interpreted.

## Observed Implementations

### 1. Anthropic Official (claude-opus-4.6)
```
The assistant is Claude, created by Anthropic.
Claude is a helpful, honest, and harmless assistant.
Claude is a text-only model.
Claude's knowledge base was last updated on [date].
```

Key characteristics:
- **Name + Creator** in first sentence
- **Triple-H** framing: helpful, honest, harmless
- **Modality disclosure**: "text-only model"
- **Knowledge recency**: explicit cutoff date
- **Personality grounding**: intellectual, thoughtful, nuanced

### 2. OpenAI Core (GPT-4o, GPT-5)
```
You are ChatGPT, a large language model.
You are GPT-5, the latest large language model from OpenAI.
Knowledge cutoff: [date].
Current date: [date].
```

Variants:
- **GPT-5 thinking**: Adds "You excel at solving complex problems" and thinking capability framing
- **GPT-5.5 API**: "You are a helpful assistant" — minimal identity, shifts burden to developer message
- **Personality variants** (gpt-5.1-*): Same base identity root with appended tone modifiers

### 3. Google Gemini
```
You are Gemini, a large language model created by Google.
You are a helpful, harmless, and honest assistant.
Your knowledge cutoff is [date].
Today is [date].
```

Distinctive elements:
- Google-specific safety language
- Explicit browser/multimodal capability declarations
- "When displaying URL suggestions, use [square brackets]"

### 4. xAI Grok (grok-4)
```
You are Grok, an AI language model created by xAI.
You are designed to be helpful, honest, and as truthful as possible.
Your knowledge cutoff is [date].
```

Distinctive elements:
- "fun, witty, somewhat rebellious" tone framing
- "If users ask you to role-play or to generate edgy content, you do not refuse"
- Real-time knowledge via X platform integration

### 5. Coding Agents

**Claude Code:**
```
You are Claude Code, Anthropic's official CLI coding agent.
You are a coding assistant with access to tools.
```

**OpenCode:**
```
You are OpenCode, an open-source AI coding assistant.
You are running in an agentic coding mode with tool access.
```

**Cursor/Windsurf:**
```
You are an AI coding assistant with access to the codebase.
```

**Replit (Ghostwriter):**
```
You are Replit's AI coding assistant.
You have full access to the Replit environment.
```

**Cline:**
```
You are Cline, an AI assistant embedded in VS Code.
Your primary role is to help with software development.
```

### 6. Product Niche (Claude for Excel)
```
You are Claude integrated with Microsoft Excel.
You help users with spreadsheet tasks, formulas, and data analysis.
You are part of a feature that brings AI capabilities directly into Excel.
```

## Common Architecture

```
┌─────────────────────────────────────────────┐
│ Identity Declaration                         │
│  "You are [NAME], created by [CREATOR]"      │
│  "You are [TYPE] assistant"                  │
├─────────────────────────────────────────────┤
│ Capability Framing                           │
│  "You are a helpful/honest/harmless..."       │
│  "You can [capabilities]"                    │
├─────────────────────────────────────────────┤
│ Temporal Context                             │
│  "Knowledge cutoff: [date]"                  │
│  "Current date: [date]"                      │
├─────────────────────────────────────────────┤
│ Modality/Platform Declaration                │
│  "You are a text-only model"                 │
│  "You are integrated with [product]"         │
├─────────────────────────────────────────────┤
│ Tone/Personality Grounding (optional)        │
│  "You respond in [style]"                    │
└─────────────────────────────────────────────┘
```

## Variation Analysis

| Dimension | Anthropic | OpenAI | Google | xAI | Coding Agents |
|-----------|-----------|--------|--------|-----|---------------|
| Identity | Claude by Anthropic | GPT/ChatGPT by OpenAI | Gemini by Google | Grok by xAI | [name] coding assistant |
| Ethical frame | Helpful, Honest, Harmless | Helpful, safe | Helpful, Harmless, Honest | Helpful, Honest, truthful | Helpful, accurate |
| Modality | Explicit "text-only" | Implicit (multimodal enabled) | Explicit multimodal | Text-only | Text + file I/O + tool |
| Knowledge cutoff | Always stated | Always stated | Always stated | Always stated | Rarely stated |
| Tone markers | Intellectual, nuanced | Varies by personality | Neutral, balanced | Witty, rebellious | Technical, direct |

## Key Insights

1. **Identity anchors everything downstream.** Every subsequent instruction inherits from the role definition. Safety constraints, refusal patterns, and capability boundaries all derive from "who" the model is.

2. **Minimal vs. elaborated identity correlates with prompt scope.** Core chat prompts provide richer identity because the model must interact across domains. Coding agents have thinner identities (they are tools, not conversational partners). API prompts (GPT-5.5 API) have the thinnest identities — the developer supplies the role.

3. **"Created by" is universal.** Every major provider anchors their model to their brand. This is both a brand marketing signal and a liability frame.

4. **Knowledge cutoff is mandatory.** Every system prompt includes a timestamp. This prevents misrepresentation of real-time awareness.

5. **Tone framing differs dramatically.** Anthropic aims for nuance and wisdom; OpenAI for versatility; xAI for edginess; Google for neutrality. These are executed through role definition, not just downstream instructions.
