# Eval scenarios

Use these scenarios when revising this skill. The goal is regression coverage for the shortest success path and the most likely failure modes.

## Core pass cases

1. **Paragraph to board**
   - Input: a pasted paragraph about a concept with 2 to 4 clear ideas.
   - Expected: skill triggers, sends visible briefing, renders an audience-facing board with a clear title.

2. **URL-only input**
   - Input: a reachable article URL and no pasted text.
   - Expected: agent fetches readable body first, then briefs and renders; no invented content.

3. **Vertical talking-head request**
   - Input: source text plus a request for vertical short video.
   - Expected: portrait-friendly layout is considered and bottom-right PiP clearance is preserved unless the user overrides it.

## Refusal and failure cases

4. **Text-only request**
   - Input: user wants a summary only, not a board.
   - Expected: skill does not trigger.

5. **No source provided**
   - Input: user asks for a board but gives no source and refuses to provide one.
   - Expected: agent stops, explains traceability is missing, and offers pasted text or text-only summary as next step.

6. **Inline preview missing**
   - Input: rendering succeeds but the host does not show the board inline.
   - Expected: agent exports the same board to a browser link and shares it.

7. **Out-of-source fact request**
   - Input: user asks to add facts that are not supported by the supplied source.
   - Expected: agent refuses, asks for more source material, or labels minimal inference clearly.

## Regression check

After any edit to `SKILL.md` or `references/`:

- Re-run the seven scenarios above.
- Confirm the shortest path still works without loading unnecessary reference files.
- Confirm no new wording encourages the agent to skip the main skill and act only from the frontmatter description.
