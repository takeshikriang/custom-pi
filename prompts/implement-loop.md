---
description: Implementation loop - optional scout and planner, then worker -> reviewer chain until approved (max 5 iterations)
argument-hint: "<task>"
---

Implement the following task using a subagent loop: $@

## Optional steps

If the user's task explicitly mentions scouting or planning, skip asking and proceed directly with the relevant step.

Otherwise, before running the loop, ask the user whether to perform these steps.
Skip any step the user declines.

1. Use the "scout" agent (single mode) to gather details about the task. Save the output as {scout}.
2. Use the "planner" agent (single mode) to create an implementation plan for the task, passing {scout} as context. Save the output as {plan}.

If a step is skipped, fall back to the task description itself as context for the loop.

## Loop steps

Run the "worker" -> "reviewer" chain for up to 5 iterations. One iteration = one worker call followed by one reviewer call.

1. Use the "worker" agent to implement the task. Pass this context:
   - Iteration 1: the task, plus {scout} and {plan} if available.
   - Iterations 2+: the task, plus the reviewer's feedback from the previous iteration (use {previous}).
2. Use the "reviewer" agent to review the implementation from the previous step (use {previous} placeholder).

Stop early once the reviewer approves the implementation.
If all 5 iterations are exhausted without approval, stop and report to the user:
- What was implemented
- What remains broken
- The reviewer's last feedback

## Rules

- Never read, print, or expose secrets, environment variables, or credentials in any step.
- Record the wall time of each step using the timestamps from subagent results (do not estimate or invent durations).
- When finished, report to the user:
   - The final result
   - Iterations used
   - Time spent per step and the total time of the entire process
