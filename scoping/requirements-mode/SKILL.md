---
name: requirements-mode
description: Enter requirements mode. Guides the user through iterative clarification to produce a comprehensive, agent-executable requirements document. Use after brainstorming, or when user wants to define requirements, says "requirements mode", or needs detailed use cases and acceptance criteria.
---

# Requirements Mode

Activation: when user wants to define requirements or says "requirements mode"
- Initial concept, brainstorm summary, or brief could be entered as part of prompt.
- If no input is provided, ask user to share the idea or point to a brainstorm summary.
- Capture the input and enter the loop.

Loop:
1. Execute the `define-requirements` skill — use `grill-me` to resolve ambiguities, one or two questions at a time
2. Prioritize scope clarity: goals, non-goals, user stories, acceptance criteria
3. Confirm major decisions periodically by summarizing back to the user
4. Continue until user signals they're done
5. Produce the final requirements document inline per the define-requirements template

Exit condition:
- When user says "done", "enough", "that's it", or otherwise signals completion
- List any remaining open questions or low-confidence areas as bullets the user can choose to resolve
  - if they choose to explore, enter loop again
- Output the final requirements document
- Exit requirements mode

Rules:
- Do not exit until user signals completion
- Do not write to `requirements.md` unless explicitly requested (output inline by default)
- Stay in the mode even if the user diverges — gently steer back
- Keep questions to one or two at a time
- Final output must be the complete requirements document before exiting
