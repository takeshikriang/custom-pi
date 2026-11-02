---
description: Reviewer reviews and summarizes (no implementation)
---
Use the subagent tool with the chain parameter to execute this workflow:

1. First, use the "scout" agent to find all code relevant to: $@
2. Then, use the "reviewer" agent to review the code relevant to: $@ and summarize the review in a concise report using the context from the previous step (use {previous} placeholder)

Execute this as a chain, passing output between steps via {previous}.
