---
description: Answer the question using the gathered context
---
Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "scout" agent to find information relevant to: $@
2. Then, use the "thinker" agent to answer the question relevant to: $@ in a clear and concise manner using the context from the previous step (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}. Do NOT implement - just relay to the user.


## Rules

- Never read, print, or expose secrets, environment variables, or credentials in any step.
- Ask for clarification whenever something is unclear or uncertain. Never make assumptions or guess.
