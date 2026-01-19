---
description: Start or resume the TDD Actor-Critic workflow
agent: orchestrator
---

Start or resume the TDD workflow.

## Current State
!`cat .tdd/state.json 2>/dev/null || echo '{"workflow_phase": "not_started"}'`

## Task Files
!`ls tasks/TDD_*.md 2>/dev/null | head -20 || echo "No task files found"`

## Instructions

1. Load the current state from `.tdd/state.json`
2. **Create TodoWrite list** with all tasks from `tasks/` directory:
   - Mark completed tasks as "completed" (from state.completed_tasks)
   - Mark current task as "in_progress" (from state.current_task)
   - Mark remaining tasks as "pending"
   - This provides real-time visibility in the UI as you work
3. Determine the next task to work on using `tdd_next`
4. **Update TodoWrite**: Mark current task as "in_progress"
5. Prepare context for the Actor agent
6. Invoke @actor to implement the task
7. Prepare context for the Critic agent (WITHOUT Actor's reasoning)
8. Invoke @critic to validate the implementation
9. Process the verdict and **update TodoWrite**:
   - APPROVED: Mark task as "completed", move to next task, mark next as "in_progress"
   - NOT APPROVED: Keep as "in_progress", retry with feedback (max 3 attempts)
10. Update state and continue until all tasks complete

**Important**: Keep the TodoWrite list updated throughout the workflow so the user can see progress in real-time.

Begin the workflow now.
