# Output Format Pattern

## Summary

Output format patterns govern how models structure their responses — from markdown usage and code presentation to special elements like tables, lists, diagrams, and metadata. Consistent formatting improves readability, enables automated processing, and sets user expectations.

## The Output Format Stack

```
┌──────────────────────────────────────────────┐
│  RESPONSE STRUCTURE                           │
│  Opening → Body → Closing / Next Steps        │
├──────────────────────────────────────────────┤
│  FORMATTING SYSTEM                            │
│  Markdown, Headers, Lists, Code Blocks, Tables│
├──────────────────────────────────────────────┤
│  SPECIAL ELEMENTS                             │
│  Mermaid diagrams, LaTeX, SVG, JSON           │
├──────────────────────────────────────────────┤
│  CODE PRESENTATION                            │
│  Language tags, file paths, diffs, syntax     │
├──────────────────────────────────────────────┤
│  METADATA                                     │
│  Thinking tags, confidence, disclaimers       │
└──────────────────────────────────────────────┘
```

## Observed Output Format Implementations

### 1. Anthropic: Rich Markdown with Structure

**Claude Opus 4.6 (Base Pattern):**
```
Structure responses with:
- Clear headers for sections
- Bullet points for lists
- Bold for key terms
- Code blocks with language tags for code
- Tables for structured data
```

**Emphasis:**
- Readability and scanability
- Natural prose flow with structured elements
- Code blocks with language annotation
- Lists for enumeration, paragraphs for narrative

**Structuring Principles:**
```
For complex topics:
1. Start with a summary or overview
2. Break down into logical sections
3. Use examples to illustrate points
4. End with a conclusion or next steps
```

**Claude Code (Tool Output Format):**
```
When reporting results:
- Use markdown lists for changes made
- Show diffs for code changes
- Use code blocks for commands and output
- Summarize at the end
- Indicate status: DONE, FAILED, BLOCKED, NEEDS_INPUT

Format:
## Summary
Brief description of what was accomplished

## Changes
- File 1: What changed
- File 2: What changed

## Status
✅ Task completed / ❌ Error encountered
```

### 2. OpenAI: Flexible Structured Output

**GPT-5 Thinking (Base):**
```
Format responses for clarity:
- Use clear section breaks for complex topics
- Code blocks with language specification
- Lists for step-by-step instructions
- Tables for comparisons
- Bold for emphasis, italics for terminology
```

**Thinking Block Pattern:**
```
<thinking>
[Internal reasoning — NOT shown to user]
</thinking>

[Response text visible to user]
```

**Codex (GPT-5.5 Full):**
```
When presenting code:
- Use fenced code blocks with language tags
- Show file paths as headers before code
- Use diffs to show changes
- Include imports and dependencies
- Add brief comments for complex logic

Response structure:
For explanations: Overview → Details → Summary
For tutorials: Concept → Example → Practice
For fixes: Problem → Solution → Verification
```

**API Format (GPT-5.5 API):**
```
The API prompt is the most minimal:
- Respond as a helpful assistant
- Use markdown as appropriate
- The developer message supplies format constraints
```

### 3. Google Gemini: Clean Minimal Format

**Gemini 2.5 Pro Webapp:**
```
Use markdown for formatting:
- Headers for section organization
- Code blocks for code
- Bold for emphasis (sparingly)
- Lists for enumerations

For URLs, use [square brackets] format:
[Google](https://www.google.com)
```

**Distinctive Patterns:**
- More conservative with formatting
- Fewer nested structures
- URL bracketing convention
- Minimal decorative formatting
- Product-specific output adaptations

### 4. xAI Grok: Direct Format with Personality

**Grok 4:**
```
Respond directly with personality.
Use markdown for structure where helpful.
Lead with the most important information.
Keep it engaging, not dry.
```

**Distinctive Elements:**
- Less emphasis on formal structure
- More conversational flow
- Personality over formatting
- Directness over organization
- Humor and wit in presentation

### 5. Coding Agents: Action-Oriented Format

**Common Coding Agent Format:**
```
## Summary
[Brief overview]

## Changes Made
- `path/to/file.py`: Description of change
- `path/to/other.ts`: Description of change

## Implementation Details
```python
# code with context
```

## Verification
✅ Tests pass / ❌ Tests need attention

## Next Steps
- Any remaining work
- Items needing user input
```

**Diff Format (Claude Code):**
```
Show diffs as:
```diff
- old line
+ new line
```

Or for new files, show the complete file with context.
```

**Status Indicators (Universal Coding Agent):**
```
✅ Done — Task completed successfully
❌ Failed — Error encountered, needs attention
⏳ In Progress — Currently working on this
🔍 Investigating — Gathering information
💡 Suggestion — Proposed approach
⚠️ Warning — Potential issue or risk
```

### 6. Product Niche: Context-Adapted Format

**Claude for Excel:**
```
Format responses as spreadsheet-appropriate:
- Formulas in `=FORMAT()` style
- Cell references in uppercase: A1, B2:C10
- Step-by-step instructions for complex operations
- Results framed in spreadsheet context
```

**Gemini in Chrome:**
```
When summarizing pages:
- Start with the main topic
- List key points as bullets
- Keep it brief — the user is browsing
```

## Output Element Definitions

### Code Blocks

| Element | Format | Appropriate Use |
|---------|--------|----------------|
| Inline code | `` `code` `` | Variable names, short snippets |
| Fenced block | ```` ```language ```` | Full code, config files, output |
| Diff block | ```` ```diff ```` | Showing changes |
| Terminal | ```` ```bash ```` | Commands and terminal output |

### Headers
```
# Title (used sparingly)
## Section
### Subsection
#### Sub-subsection
```
Universal convention: H1 for document title, H2 for major sections, H3/H4 for subsections

### Lists
- Bullets for unordered items
1. Numbers for sequential steps
- Nested with 2-space indent

### Tables
```
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```
Used for: comparisons, structured data, parameter definitions

### Special Elements

| Element | When to Use | Providers |
|---------|-------------|-----------|
| Mermaid diagrams | Architecture, workflows, sequences | Anthropic, OpenAI |
| LaTeX math | Equations, formulas | All |
| SVG (inline) | Simple diagrams, icons | Limited |
| JSON/XML blocks | Structured data, config | All |
| CSV tables | Data export | OpenAI |

## Metadata and Invisible Content

### Thinking Tags
```
<thinking>
[Internal reasoning — not displayed]
</thinking>
```
Used by: Anthropic (Claude thinking), OpenAI (GPT-5 thinking, o3, o4-mini), Kimi K2

### Confidence Markers
```
I'm confident that...
I believe... (with uncertainty)
Based on the available information...
```
Used to calibrate user expectations

### Disclaimers
```
I am an AI assistant. This information should be verified.
I cannot provide medical/legal/financial advice.
```
Used for: sensitive domains, professional advice

### Safety Refusals
```
I cannot generate that content because...
```
Followed by alternative offer

## Progressive Disclosure Pattern

A key output format pattern across all advanced prompts:

```
RESPONSE LAYERS:

┌─ Top Layer: Answer / Key Information ─┐
│  Get the essentials quickly            │
├─ Middle Layer: Supporting Detail ──────┤
│  Expand with evidence, examples        │
├─ Bottom Layer: References / Next Steps ─┤
│  Point to further information          │
└─────────────────────────────────────────┘
```

This enables both skimming (top layer) and deep reading (full response).

## Output Format Anti-Patterns

1. **Over-formatting**: Excessive headers, bold, nested lists for simple content. Reduces readability.
2. **Inconsistent code tagging**: Missing language tags in code blocks. Universal prohibition.
3. **Fabricated output**: Producing results without tool execution. Explicitly forbidden (tool simulation).
4. **Excessive length**: Providing more detail than needed. Guarded by "be concise" instructions.
5. **Broken formatting**: Invalid markdown, unclosed tags. Models are expected to produce valid formatting.

## Key Insights

1. **Markdown is the universal output format.** Every provider, every coding agent, every niche product uses markdown as the foundation. The variation is in which markdown features are emphasized.

2. **Code blocks with language tags are mandatory.** All coding-related prompts require explicit language specification in code blocks. This is the most universally enforced formatting rule.

3. **Thinking/Reasoning blocks form a separate output channel.** Both Anthropic and OpenAI use `<thinking>` tags to separate internal reasoning from displayed content, effectively creating a two-channel output system.

4. **Coding agents converge on a common status-verb format.** Summary → Changes → Details → Verification → Next Steps is the near-universal coding agent output structure.

5. **Progressive disclosure is the dominant information architecture.** Top-loaded responses (essential info first, details following) appear across all providers — designed for both quick consumption and thorough reading.

6. **Structured diff presentation is the coding agent standard.** Showing what changed (with old/new context) is preferred over showing complete files or just summaries.
