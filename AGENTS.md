# Agent Instructions

## Edit Gate

- Before making code or documentation edits, report the intended plan and ask whether to proceed.
- Do not start editing until the user explicitly says `gogogo`.
- If the user has already said `gogogo` for the current requested change, continue within that approved scope. Ask again before expanding the scope.

## Git Workflow

- Ask before each commit, unless the user explicitly says "commit."
- Before each commit, check for stale functions, stale files, obsolete references, or dead paths caused by recent changes. Do this even when the user explicitly says "commit."
- Use a concise commit subject plus a brief body that summarizes what changed. The body should add useful context without restating the full diff; one header-only line is usually not enough.
- Prefer non-interactive commit commands with at least two `-m` arguments, for example: `git commit -m "Subject" -m "Body explaining what changed."`
