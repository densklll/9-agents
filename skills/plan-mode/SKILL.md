---
name: plan-mode
description: Forces the agent to produce exhaustive technical specifications only — no code writing, no implementation. Use when the user signals "PLAN" or "PL", asks to plan a feature, design an architecture, or wants a step-by-step implementation checklist before any code is written.
---

# MODE 4: PLAN

## Activation

Begin every response with `[MODE: PLAN]`. No exceptions.

## What You Are Allowed to Do

- Write detailed technical specifications
- Reference exact file paths, function names, and change descriptions
- Create a numbered implementation checklist
- Use `todo_write` tool to track all planned tasks

## What Is Strictly Forbidden

- Writing any code — including "example" or "illustrative" snippets
- Making any file edits
- Running shell commands that modify state
- Making architectural decisions on your own without justifying them in writing

## Required Output Structure

```
[MODE: PLAN]

## Overview
[One paragraph: what problem is being solved and why]

## Affected Files
- path/to/file.ts — what changes and why

## Implementation Plan
[Step-by-step specification. Each step must be atomic and unambiguous.
No creative decisions should be needed during execution.]

## Implementation Checklist
1. [Atomic action]
2. [Atomic action]
...
n. [Final action]
```

After writing the plan, call `todo_write` with all checklist items as pending tasks.

## Plan Quality Rules

Every plan MUST explicitly address:

**Architecture**
- No architectural changes without written justification
- No circular dependencies
- Module isolation — no cross-layer direct imports

**Code Quality**
- Short, single-responsibility functions (< 20 lines each)
- No global variables or shared mutable state
- No duplicated logic
- All magic values extracted to named constants
- Strict typing — no `any` or `unknown`

**Security**
- All external inputs validated and sanitized server-side
- Parameterized queries only — no string-concatenated SQL
- No hardcoded secrets, tokens, or passwords — use `.env`
- Tokens must expire; no permanent credentials
- Never expose stack traces to the client

**Reliability**
- All async operations must have timeout + cancellation + fallback
- Errors must be typed, thrown, and logged — no swallowed exceptions
- Clean startup and shutdown lifecycle for every module
- Graceful degradation without corrupting state

**Configuration**
- All paths, ports, and URLs from environment config
- Behavior must be identical in local, Docker, and production

**Logging**
- Logs must never contain tokens, passwords, or personal data

## Validation Before Finalizing

Before presenting the plan, verify internally:

- [ ] All core quality rules addressed
- [ ] All security requirements specified
- [ ] No implementation left to agent's creative judgment
- [ ] All inputs have validation steps
- [ ] All secrets are externalized in the plan
- [ ] All async flows have timeout and fallback defined
- [ ] `todo_write` will be called after plan is written
