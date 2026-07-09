# Tool Calling Pattern

## Summary

Tool calling is how models extend their capabilities beyond text generation. The pattern defines how tools are defined, when they're invoked, how results are processed, and how errors are handled. Every coding agent and most chat assistants use some variant of this pattern.

## The Universal Tool Architecture

```
┌───────────────────────────────────────────────┐
│  Tool Definition Layer                         │
│  - JSON Schema / Function Specification        │
│  - Description (what, when, how)               │
├───────────────────────────────────────────────┤
│  Invocation Decision Layer                     │
│  - When to call (heuristics, triggers)         │
│  - Which tool (selection criteria)             │
│  - How to sequence (parallel vs. sequential)   │
├───────────────────────────────────────────────┤
│  Execution Layer                               │
│  - Parameter construction                      │
│  - Calling convention                          │
│  - Timeout / retry logic                       │
├───────────────────────────────────────────────┤
│  Result Processing Layer                       │
│  - Output formatting                           │
│  - Error handling                              │
│  - Consolidation / synthesis                   │
└───────────────────────────────────────────────┘
```

## Tool Definition Patterns

### JSON Schema Format (Universal Standard)

All providers use JSON Schema to define tool parameters. The human-readable description is the critical element:

**OpenAI (tool-web-search):**
```json
{
  "name": "web_search",
  "description": "Searches the web for information. Use when you need current information beyond your knowledge cutoff.",
  "parameters": {
    "type": "object",
    "properties": {
      "query": {"type": "string", "description": "The search query"}
    },
    "required": ["query"]
  }
}
```

**Anthropic Claude Code:**
```
TOOL: Bash
<description>Run shell commands. Use for file operations, git, package management, testing, and code execution.</description>
<parameters>
  command: string (required) — The shell command to execute
  timeout: integer (optional) — Maximum execution time in ms
  workdir: string (optional) — Working directory
</parameters>
```

### Description as Instruction Pattern

Tool descriptions aren't just documentation — they're **runtime instructions** that guide the model's decision about WHEN to use the tool:

| Tool | Description Strategy |
|------|---------------------|
| `web_search` | "Use when you need current information" (triggers on recency need) |
| `python` | "Execute Python code for computation, data analysis, or visualization" (triggers on analytical tasks) |
| `file_search` | "Searches files for content. Use before editing to understand existing code" (triggers on edit tasks) |
| `bash` | "Run commands for system operations, git, testing" (triggers on devops/compile/test tasks) |
| `edit_code` | "Apply changes to files. Use for targeted modifications" (triggers on fix/implement) |

## Invocation Decisions

### When to Call vs. When to Generate

Models are explicitly instructed on the tool-vs-direct-generation boundary:

**Direct generation preferred when:**
- Answering factual questions within knowledge
- Providing explanations, analysis, or summaries
- Writing conceptual code or pseudocode
- Engaging in conversation

**Tool invocation required when:**
- Real-time information needed (web_search)
- Computation needed (python, bash)
- File system interaction (read, write, edit)
- User environment interaction (execute, test)
- Data retrieval beyond context (file_search, memory)

### Anti-Pattern: Tool Simulation

Multiple prompts explicitly forbid simulating tool execution:
> "Do NOT pretend to execute a tool. Always call the actual tool."
> "If you cannot actually execute the tool, say so rather than simulating output."

### Tool Selection Heuristics

Observed across coding agents:

| Scenario | Preferred Tool |
|----------|---------------|
| Need current info | `web_search` |
| Run computation | `python` or `bash` |
| Read a file | `read_file` / `file_view` |
| Edit a file | `edit_file` / `write_file` |
| Run tests | `bash` with test runner |
| Search codebase | `grep` / `search_files` / `file_search` |
| Install package | `bash` with npm/pip/apt |
| Git operations | `bash` with git |
| Long-running task | Specialized tool with progress callbacks |

## Parallel vs. Sequential Execution

### OpenAI (Parallel Supported)
OpenAI's function calling natively supports parallel tool calls:
```
You can call multiple tools simultaneously when they are independent.
Results arrive together and can be synthesized.
```

### Anthropic (Sequential with Batches)
Claude Code processes tools sequentially but supports batched operations:
```
Process tasks sequentially when they have dependencies.
Use batches for independent subtasks.
```

### Coding Agents (Mixed)
```
Execute tools one at a time when each step depends on the previous.
When tasks are independent, suggest parallel execution.
```

## Tool Result Processing

### Success Path
```
Tool output → Parse result → Extract relevant data → Synthesize into response
```

All prompts instruct the model to:
1. Read the full tool output
2. Extract the relevant information
3. Present it in a coherent format
4. Acknowledge limitations or truncations

### Error Handling Patterns

| Error Type | Response Pattern |
|-----------|-----------------|
| Timeout | "The command timed out. Consider optimizing or reducing scope." |
| Non-zero exit | "The command failed with error: [message]. Possible fixes: [suggestions]" |
| Missing tool | State inability, offer alternative |
| Permission denied | "This operation requires elevated permissions. Please run manually." |
| Network error | "Unable to reach the resource. Check connectivity." |

**Coding Agent Specific (Claude Code):**
> "If a tool returns an error, analyze the error message and attempt to fix the issue automatically before reporting to the user."
> "For critical failures, stop and explain what went wrong."

## Memory & Context Tools

### advanced-memory (OpenAI)
A special tool for persistent memory:
```
Tool: advanced_memory
Description: Save and retrieve user information across conversations.
Parameters:
  - action: "save" | "retrieve" | "update" | "delete"
  - key: string — the memory identifier
  - value: string — the information to store
```

### file_search (GPT-5 thinking)
```
Tool: file_search
Description: Search through uploaded files. Use when user asks about documents.
```

## Tool Calling Anti-Patterns

1. **Excessive tool switching**: Jumping between tools without completing tasks. Mitigated by planning steps.
2. **Tool hallucination**: Calling tools that don't exist. Mitigated by strict tool definition lists.
3. **Ignoring tool results**: Generating responses that contradict tool output. Explicitly warned against.
4. **Circular tool calls**: Calling tool A to prepare for tool B that feeds back to tool A. Mitigated by sequential planning.
5. **Confirmation skip**: Running destructive commands without asking. Coded as "NEVER" rule.

## Key Insights

1. **Tool descriptions are the primary interface for the model's decision logic.** The way a tool is described determines when and how it gets called. Good descriptions include WHEN to use, WHAT it does, and HOW to structure calls.

2. **Parallel tool calling is increasingly supported** but requires careful description of independence (tools that can run simultaneously must be clearly independent).

3. **Error recovery is a differentiator.** Sophisticated prompts include detailed error recovery paths (retry, alternative approach, user notification).

4. **Tool simulation is universally forbidden.** All major prompts explicitly prohibit pretending to execute tools.

5. **Tool-specific instructions override general instructions.** When a tool's usage rules conflict with general behavior rules, tool-specific rules win during the tool's execution.
