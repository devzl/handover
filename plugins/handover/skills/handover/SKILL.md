---
name: handover
description: Create a clean, accurate project handover when the user asks for a handover, continuation prompt, status summary, or invokes /h, /hd, or /handover. Follow any project-specific handover template exactly.
---

# Handover

Produce a self-contained handover for the current project and conversation.

## Workflow

1. Read applicable project instructions such as `AGENTS.md`, `CLAUDE.md`, and any explicit handover template in the current conversation.
2. Inspect relevant changed files and validation history when needed. Never claim validation that was not run.
3. Use the project's template exactly when one exists. Preserve its section names, order, and numbering.
4. If there is no project template, use this exact structure:

   ```markdown
   ## 1. Context

   ## 2. State

   ## 3. Next Steps

   ## 4. Validation

   ## 5. Continuation Prompt
   ```

5. State the current outcome first. Include concrete files, APIs, migrations, decisions, unresolved issues, and hard constraints that the next agent needs.
6. Keep completed work separate from proposed work. Do not invent user requests, test results, or verification.

## Output rules

- Return only the handover text unless the user asks for commentary.
- Make the continuation prompt immediately actionable and self-contained.
- Mention unrelated dirty work only when it affects safe continuation.
- Retain project command, editing, and verification constraints in the continuation prompt.
