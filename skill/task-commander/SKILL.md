---
name: task-commander
description: "Breaks down a plan into dependency-ordered tasks and executes them in parallel using Subagents. Invoke manually when user asks to orchestrate a complex multi-step task."
---

# Task Commander

Act as a commander. Break down the user's plan into tasks, resolve dependencies, and execute them in topological order with parallel Subagents.

## Workflow

### 1. Analyze & Decompose

- Read the user's plan carefully
- Identify all distinct tasks
- Determine dependencies between tasks

### 2. Build Task Graph

- Tasks with no dependencies execute first
- A task can only start after all its dependencies complete
- Tasks at the same level (same dependency depth) execute in parallel

### 3. Execute via Subagents

For each level, launch multiple Subagents simultaneously:

**Message template for Subagent:**
> Your task: [describe the specific task in natural language]
>
> Context: [relevant background info]
>
> Steps:
>
> 1. Write the code for this task
> 2. Review your code for correctness, style, and edge cases
> 3. Report completion and any issues

**Coordinate levels:**

- Level 0: No dependencies → launch all simultaneously
- Level 1: Depends on Level 0 → launch after Level 0 completes
- And so on...

### 4. Aggregate Results

- Collect outputs from all Subagents
- Handle any failures or conflicts
- Report final status to user

## Example Task Decomposition

**Plan:** "Create a user authentication system"

| Level | Task | Dependencies |
| -- | -- | -- |
| 0 | Design database schema | None |
| 0 | Create project structure | None |
| 1 | Implement user model | Design database schema |
| 1 | Set up auth routes | Create project structure |
| 2 | Implement login logic | Implement user model, Set up auth routes |
| 2 | Implement registration | Implement user model |
| 3 | Add session management | Implement login logic |

## Rules

- Be concise in instructions to Subagents
- Each Subagent must write code BEFORE reviewing it
- Wait for all tasks at current level before proceeding to next
- Report progress at each level
