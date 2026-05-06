# Skills

Purpose:
- Markdown-only repository of reusable skills/prompts.
- Used by traikker as system skills

Folder structure:
- structure aligns with traikker workflows
    - Scoping: brainstorm -> requirements
    - Execution: planning -> development
    - Review: publish
- general purpose skills go into general/
- all other skills go into scratch/ by default

Behavior:
- No runtime code here.
- folder structure may not change except with explicit user permission
- Preserve any frontmatter or format expected by the consuming agent system.

When editing:
- Keep skills concise, directive, and outcome-oriented.
- Any markdown file a skill writes for a target project must be placed under `/<project-root>/.agent/` (e.g., `<project-root>/.agent/CONTEXT.md`, `<project-root>/.agent/requirements.md`).
