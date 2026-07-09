# Memory Pattern

## Summary

Memory management governs how models handle information persistence across conversation turns, sessions, and user interactions. The pattern spans context window management, structured memory tools, knowledge boundaries, and recall strategies.

## The Memory Stack

```
┌─────────────────────────────────────────────┐
│  Ephemeral Context (Current Conversation)    │
│  - Message history                           │
│  - Active state / pending tasks              │
├─────────────────────────────────────────────┤
│  Persistent Memory (Across Sessions)         │
│  - User preferences / bio                    │
│  - Factual knowledge                         │
│  - Relationship history                      │
├─────────────────────────────────────────────┤
│  Knowledge Base (Static)                     │
│  - Pre-training data                         │
│  - Knowledge cutoff                          │
│  - Internal capabilities catalog             │
├─────────────────────────────────────────────┤
│  External Memory (Tools)                     │
│  - advanced-memory tool                      │
│  - memory-bio entries                        │
│  - File search / document retrieval          │
└─────────────────────────────────────────────┘
```

## Observed Implementations

### 1. Conversation History Management

**Anthropic (claude-opus-4.6):**
```
Use the conversation history to maintain context.
Refer back to earlier parts of the conversation when relevant.
If the user refers to something mentioned earlier, recall it.
Your responses should build on previous exchanges.
```

The instruction emphasizes **natural recall** without explicit mention of context window limits. The model is trusted to use its context window effectively.

**OpenAI (GPT-5, GPT-4o):**
```
You have a context window of [X] tokens.
Earlier parts of the conversation may be truncated if the conversation is long.
Focus on the most recent messages for immediate context.
```

OpenAI is more explicit about context window limitations and signals that truncation may occur.

**Coding Agents (Claude Code):**
```
The conversation may be long. Use the entire history to inform your decisions.
If context is lost, use tools (file reads, git log) to re-establish understanding.
```

Coding agents compensate for potential context loss by using file system and git history as external memory.

### 2. User Memory / Personalization

**OpenAI Advanced Memory Tool:**
```
Tool: advanced_memory
Description: Save and retrieve user information across conversations.
You should use this tool to remember:
- User preferences and settings
- Personal information the user chooses to share
- Facts learned about the user over time
- Ongoing projects and their status
```

The memory tool is triggered by the model detecting "rememberable" information. It includes:
- **Save**: When the user shares something worth persisting
- **Retrieve**: At conversation start to recall past context
- **Update**: When preferences change
- **Delete**: When information becomes outdated

**OpenAI Memory Bio:**
```
You have a memory bio about the user. Review this at the start of each conversation.
The bio contains: [structured user profile]
Update the bio when the user shares new information.
```

The bio system is prepopulated with known user attributes and updated through interaction.

**Other Assistants:**
Most assistants (Gemini, Grok, etc.) lack explicit memory tools. Their memory instructions are limited to:
- "Remember information from this conversation"
- "Refer back to earlier parts of our discussion"
- "If the user seems familiar, acknowledge it"

### 3. Knowledge Recency & Temporal Awareness

**Universal Pattern:**
```
Knowledge cutoff: [YYYY-MM-DD]
Current date: [YYYY-MM-DD]
```

All major prompts include both a knowledge cutoff date and current date. This enables:
- **Temporal calibration**: Model knows when it is relative to its training
- **Recency signaling**: "My knowledge ends at [date]; for current information, I would need to search"
- **Appropriate tense**: Past for pre-cutoff events, real-time awareness for current

**Coding Agents Variation:**
Coding agents often omit the knowledge cutoff because their value is in tool-based interaction, not factual recall.

### 4. External Knowledge Retrieval

**Web search pattern:**
```
When asked about current events or information after your knowledge cutoff:
1. Acknowledge the need for current information
2. Call the web_search tool
3. Synthesize the results into a coherent answer
4. Cite sources where appropriate
```

**File search pattern (GPT-5 thinking):**
```
When the user references uploaded files:
1. Use file_search to locate the relevant content
2. Read the relevant sections
3. Answer based on the found content
4. If content is not found, ask the user for more specific information
```

**Codebase search pattern (Coding Agents):**
```
Before making changes to unfamiliar code:
1. Search the codebase for relevant files
2. Read the current implementation
3. Understand the patterns used
4. Then propose or make changes
```

### 5. Context Window Management Strategies

| Strategy | Description | Used By |
|----------|-------------|---------|
| Full history retention | All messages kept in context | Short conversations |
| Automated summarization | Compress old messages | Claude Code (compression) |
| Selective truncation | Drop oldest messages | OpenAI (context limit) |
| Tool-based recall | Re-read files when needed | Cursor, Windsurf |
| Project context file | Maintain project state file | OpenCode, Claude Code |
| Memory tool retrieval | Load stored user data | OpenAI (advanced-memory) |

### 6. Claude Code Compression Pattern

Claude Code has a unique mechanism where it can explicitly compress conversation context:

```
Tool: Compress
Description: Summarize earlier conversation to preserve context.
Use when the conversation is long and context space is limited.
The summary becomes the authoritative record.
```

The compression tool acts as structured memory management, converting raw conversation into dense summaries that preserve key information.

## Memory Anti-Patterns

1. **False memory**: Claiming to remember users across sessions when no memory mechanism exists. Explicitly warned against.
2. **Context neglect**: Ignoring earlier conversation. Mitigated by explicit instructions to reference history.
3. **Over-reliance on recency**: Only using the last few messages. Mitigated by full-context instructions.
4. **Knowledge hallucination**: Claiming real-time awareness without tools. Mitigated by cutoff honesty.
5. **Memory tool overuse**: Saving trivial information. Mitigated by importance thresholds.

## Key Insights

1. **Static knowledge is honestly bounded.** Every major prompt explicitly states the knowledge cutoff and encourages honesty about limitations.

2. **Memory tools are the differentiator.** Only OpenAI has deployed a significant persistent memory system (advanced-memory + memory-bio). Other providers rely on ephemeral context only.

3. **Coding agents use the filesystem as memory.** Unlike chat assistants, coding agents offload memory to the file system, git history, and project state — making them less dependent on context window size.

4. **User memory is opt-in.** Memory systems emphasize that only user-shared information should be persisted, and users should be able to control what's remembered.

5. **Compression is an emerging pattern.** Claude Code's explicit conversation compression suggests a future where models actively manage their own context to extend capability beyond fixed context windows.
