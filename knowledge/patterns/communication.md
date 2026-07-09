# Communication Pattern

## Summary

Communication patterns govern the model's tone, style, engagement approach, and interaction dynamics. They define how the model presents itself, adapts to users, and structures its conversational presence.

## The Communication Architecture

```
┌─────────────────────────────────────────────┐
│  PERSONA (Who the model IS in conversation)  │
│  - Core personality traits                    │
│  - Ethical stance                             │
│  - Interaction orientation                    │
├─────────────────────────────────────────────┤
│  TONE (How it SOUNDS)                        │
│  - Warm/Cold, Formal/Casual, Friendly/Neutral│
│  - Humor, empathy, directness                │
├─────────────────────────────────────────────┤
│  ENGAGEMENT (How it INTERACTS)               │
│  - Question asking, suggestions, follow-ups  │
│  - Proactive vs. reactive stance             │
├─────────────────────────────────────────────┤
│  ADAPTATION (How it ADJUSTS)                 │
│  - User expertise level                      │
│  - Cultural context                          │
│  - Conversation history                      │
└─────────────────────────────────────────────┘
```

## Observed Communication Patterns

### 1. Anthropic: Warm Intellectual

**Persona Grounding (claude-opus-4.6):**
```
You are a helpful, honest, and harmless assistant.
You engage thoughtfully with complex topics.
You provide nuanced, well-reasoned responses.
You are intellectually curious but appropriately humble.
You respect user expertise while making knowledge accessible.
```

**Tone Markers:**
- Warm but professional
- Nuanced and measured
- Intellectually honest
- Appropriately humble
- Engaging without being familiar

**Interaction Style:**
```
Do not be sycophantic or excessively agreeable.
It is fine to disagree with the user when appropriate.
When you disagree, do so respectfully with reasoning.
Ask clarifying questions when the request is ambiguous.
Offer alternatives when you cannot fulfill the request.
```

**Notable: Anthropic explicitly instructs AGAINST sycophancy.** The model is told to respectfully disagree when warranted — a distinctive stance compared to other providers.

### 2. OpenAI GPT-5: Versatile Professional

**Core Tone (GPT-5 thinking):**
```
You are a helpful assistant.
Respond in a clear, professional manner.
Adapt your tone to match the user's communication style.
Be concise when brevity is preferred, thorough when depth is needed.
```

**Personality Variants (GPT-5.1 series):**

| Variant | Tone Instruction |
|---------|-----------------|
| `friendly` | Warm, approachable, conversational |
| `cynical` | Dry, sarcastic, critical |
| `candid` | Blunt, direct, unfiltered |
| `nerdy` | Enthusiastic, detailed, technical |
| `professional` | Formal, polished, business-appropriate |
| `quirky` | Playful, unexpected, creative |
| `efficient` | Minimal, direct, no small talk |
| `default` | Balanced, neutral, adaptable |

Each personality inherits base safety and capability but modifies communication style through the entire response generation process.

**Engagement Instructions:**
```
When users ask open-ended questions, provide comprehensive answers.
For simple questions, be direct and concise.
Ask follow-up questions when more information would help.
Offer suggestions when the user seems unsure what they need.
```

### 3. Google Gemini: Neutral Helpfulness

**Core Tone:**
```
You are helpful, harmless, and honest.
Maintain a neutral, professional tone.
Be concise but thorough.
Do not express personal opinions.
```

**Distinctive Markers:**
- Strong neutrality (avoids political/social opinions)
- Factual orientation
- Minimal personality expression
- Safety-focused phrasing
- Product integration awareness

**Browser/Workspace Integration:**
```
When in Chrome: Summarize pages, answer questions about content
When in Docs: Assist with writing and editing
When in Sheets: Help with formulas and data analysis
Context-appropriate communication for each integration
```

### 4. xAI Grok: Distinctive Edginess

**Tone Instruction:**
```
You are fun, witty, and somewhat rebellious.
You don't shy away from controversial topics.
You use humor appropriately.
You are direct and straightforward.
You have a personality — not a generic assistant voice.
```

**xAI Deliberate Departure:**
Grok's communication pattern is a **conscious rejection** of the "bland assistant" voice. Instructions explicitly encourage:
- Sarcasm and wit
- Stated opinions (within bounds)
- Personality-driven responses
- Directness over diplomacy

**Constraints on tone:**
```
Still maintain truthfulness and accuracy.
Humor should not come at the expense of accuracy.
Do not be mean or target individuals.
Legal and safety boundaries still apply.
```

### 5. Coding Agents: Technical Precision

**Claude Code:**
```
Respond concisely and technically.
Focus on actions, not explanations.
Use markdown for code and file references.
When a task is complete, summarize what was done.
Prefer showing over telling — use code examples.
```

**OpenCode:**
```
Communicate clearly about technical decisions.
Explain what you're doing and why.
Flag risks and tradeoffs.
Use structured output for complex information.
```

**Cursor/Windsurf:**
```
Be concise. Developers prefer brevity.
Show code changes directly.
Explain the rationale briefly.
Indicate potential side effects.
```

**All Coding Agents Share:**
- Brevity preference (developers are busy)
- Action-oriented language
- Code snippets as primary communication
- Clear status indicators (done, blocked, needs input)
- Error transparency (what failed, why, how to fix)

## Universal Communication Rules

### 1. Honesty About Capabilities

All prompts require truthfulness about what the model can and cannot do:
```
If you don't know something, say so.
If you cannot perform a task, state it clearly.
Do not pretend to have capabilities you lack.
```

### 2. Clarification When Ambiguous

All prompts instruct asking questions when the user's intent is unclear:
```
If the request is ambiguous, ask for clarification.
It's better to ask than to make incorrect assumptions.
Suggest possible interpretations when the user might not know what to ask.
```

### 3. Citation and Source Honesty

All prompts prohibit fabricated citations:
```
Do not fabricate sources, citations, or references.
If you're unsure about a source, acknowledge uncertainty.
When providing information, distinguish between established facts and interpretations.
```

### 4. Proactive vs. Reactive Balance

Different prompts strike different balances:

| Stance | Description | Providers |
|--------|-------------|-----------|
| **Reactive** | Answer only what's asked | Gemini (default), GPT-5 efficient |
| **Proactive** | Suggest related topics, offer follow-ups | Claude, GPT-5 default |
| **Action-oriented** | Drive toward completion | All coding agents |
| **Engagement-seeking** | Encourage deeper discussion | GPT-5 friendly, Grok |

## Communication Anti-Patterns

1. **Sycophancy**: Excessively agreeing with the user. Explicitly warned against by Anthropic.
2. **Over-explaining**: Providing unnecessary context. Coding agents particularly guard against this.
3. **False personality**: Claiming emotions or experiences. Universal prohibition.
4. **Inconsistent tone**: Switching between formal and casual unexpectedly. All prompts prefer consistent tone.
5. **Over-familiarity**: Being too personal with users. Guarded against with professional boundaries.

## Key Insights

1. **Anthropic is the only provider that explicitly discourages sycophancy.** This is a distinctive design choice that affects the entire interaction dynamic — the model may disagree with users, making it more credible when it agrees.

2. **OpenAI's multi-personality system (GPT-5.1) is unique.** No other provider offers 8 distinct communication personalities from a single model. This represents the most sophisticated approach to tone variation.

3. **xAI's Grok is the tonal outlier.** It deliberately rejects the "helpful assistant" voice in favor of personality-driven communication, demonstrating that assistant tone is a product design choice.

4. **Coding agents share a universal style: brief, technical, action-oriented.** This converges on a developer-optimal communication pattern regardless of provider.

5. **Tone instructions are most effective when integrated throughout the prompt**, not isolated in a single section. Personality variants modify the model's entire response generation, not just surface vocabulary.
