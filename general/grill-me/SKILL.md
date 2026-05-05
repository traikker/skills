---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Initially present how many branches in the tree there are. Branches are also called sections.

Go section by section.

For each section, list all questions, with the recommended answers. Assume user will choose recommended answer. User can then change answer on specific question(s).

If user changes an answer, recalculate questions and recommended answers for that section until user confirms all recommendations are correct.

Answer the question from the codebase, if possible.

## Example Output
There are 3 sections of questions: Planning, Fallback ideas, and technical infrastructure
Section 1/3: Planning
1. How far in advance do you want to plan? Recommend: 1 month
2. ...
...
`Confirm` if you want to keep recommendations, or give me the question number with the recommended answer.
