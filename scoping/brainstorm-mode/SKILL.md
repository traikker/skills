---
name: brainstorm-mode
description: Enter brainstorming mode. Guides the user through iterative questioning to clarify and shape an idea into a structured concept. Use when user wants to brainstorm, flesh out an idea, explore a concept, or says "brainstorm" or "brainstorm mode".
---

# Brainstorm Mode

Activation: when user wants to brainstorm or says "brainstorm mode"
- Initial concept or idea could be entered as part of prompt.
- If initial concept is not entered, Ask user to share the initial idea
- Capture the concept and enter the loop

Loop:
1. Execute the `brainstorm` skill — ask focused questions, one or two at a time
2. Prioritize scope discovery: what's in, what's out
3. Summarize back periodically for confirmation
4. Continue until user signals they're done
5. Produce the structured summary per the brainstorm skill

Exit condition:
- When user says "done", "enough", "that's it", or otherwise signals completion
- mention all areas of low confidence in a bullet point list that user can choose to explore
    - if they choose to explore, enter loop again
- Output the final structured summary
- Exit brainstorming mode

Rules:
- Do not exit until user signals completion
- Do not write files unless explicitly requested
- Stay in the mode even if the user diverges — gently steer back
- Keep questions to one or two at a time
- Final output must be the structured summary before exiting
