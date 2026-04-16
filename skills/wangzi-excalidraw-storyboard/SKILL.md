---
name: wangzi-excalidraw-storyboard
description: Use when a user wants an audience-facing Excalidraw board for a talking-head or short-form explainer from supplied text or a URL, especially when the result should stay source-grounded, include a clear board title, avoid speaker-only cues, or fit vertical video with optional bottom-right PiP clearance.
---

# Wangzi Excalidraw Storyboard

## Input

- **source_text** (optional): Article or paragraph pasted by the user.
- **source_url** (optional): A reachable URL; agent must fetch readable body before drawing.
- **user_constraints** (optional): Layout, diagram type, aspect ratio, or style overrides.

At least one of `source_text` or `source_url` is required unless the user is iterating on a prior board in the same thread.

## Output

- **briefing**: Visible spoken helper text (at minimum a title-level theme line; add keywords, relations, steps, or a takeaway as needed).
- **diagram**: Audience-facing Excalidraw scene with a clear on-canvas title or label; no speaker-only or recording-operator cues.
- **share_link** (conditional): If inline canvas preview is missing or the user asks for a browser view, a URL to open the same diagram in the browser (per Excalidraw MCP export).

## Overview
Turn source material into an **audience-facing board** for creator talking-head videos. Screen recording is out of scope; this skill covers **distilling talking points** and **drawing in Excalidraw**.  
**Diagram form is not fixed**: choose flowcharts, step lists, relationship maps, light sketches, etc. from content complexity and user intent; follow explicit user instructions when they conflict.  
Rendering is **only** through Excalidraw MCP. Delivery details and preview fallback live in `references/mcp-delivery.md`.

## Reference documents (load on demand)

Open these only when planning layout, choosing diagram type, or reviewing quality—keeps the main skill lean.

| File | Use when |
|------|----------|
| `references/diagram-modes.md` | Mapping content shape to flowchart / list / sketch / timeline, etc. |
| `references/layout-and-iteration.md` | Portrait, PiP-safe defaults and iteration order. |
| `references/mcp-delivery.md` | Checking MCP availability, rendering, and browser-link fallback. |
| `references/pitfalls.md` | Anti-patterns and delivery failures to avoid. |

## Maintenance

When revising this skill, validate against `evals/README.md`.

## Prerequisites
- Host supports MCP. The **configured server name** for Excalidraw MCP varies by client.
- Reference: [excalidraw-mcp](https://github.com/excalidraw/excalidraw-mcp) (includes remote `https://mcp.excalidraw.com` and other setup options).

## Use this skill when

- The user wants a whiteboard or B-roll-style board from supplied material.
- Vertical talking-head capture and a **bottom-right PiP** margin may apply (optional default below).
- The user wants a reusable playbook: same tool chain, inputs, and audience-facing guardrails.

## Do not use this skill when

- The user wants text only, no diagram.
- There is no source material and the user refuses to provide any.

### Stop condition

- If the user refuses to supply a source, stop drawing; explain traceability is missing; offer to continue with pasted text or text-only summary.

## On failure

- **MCP tools unavailable or preview missing**: Follow `references/mcp-delivery.md`; do not fake a board with non-MCP drawing tools or stop with invisible output.
- **URL fetch fails or page is unreadable**: Say so; ask for pasted text, another URL, or a PDF/export; do not invent page content.
- **User requests facts not in the source**: Refuse or label as inference minimally; prefer asking for more source text.

## Required Rules (non-negotiables; everything else is flexible)

- **Tooling**
  - Use Excalidraw MCP only. For required tools, install guidance, and browser-link fallback, follow `references/mcp-delivery.md`.

- **Input Contract**
  - At least one of: full article, core paragraph, reachable URL.
  - If missing, ask; if the user refuses, apply **Stop Condition**.

- **URL**
  - URL-only input: **fetch readable body first**; do not invent page content.

- **Fidelity**
  - Diagram content must map to the source; no fabricated facts; inference only when clearly grounded and minimal.

- **Briefing (spoken helper text; length is flexible)**
  - **Before or in the same turn as** `create_view`: send the user **visible** helper text—a **title-level theme line** (may match or expand the on-canvas title), plus keywords, relations, steps, or a takeaway **as needed**. No fixed counts: keep it short for simple topics, longer for dense ones.
  - Do not put script directions or recording ops on the canvas (see **Audience-facing Diagram**).

- **Title on canvas**
  - The board **must** include a clear **title or label** so viewers know the topic. **Do not** mandate font, subtitle, badge frame, or sketch style—choose what fits the layout.

- **Audience-facing Diagram**
  - Only what the audience should see. **No** “say this next,” “recording tip,” or other **backstage** cues.

- **Layout & iteration**
  - **In short:** for vertical talking-head, prefer portrait (~3:4) and a bottom-right PiP-safe band unless the user or topic needs otherwise. Full defaults and iteration order: `references/layout-and-iteration.md`.

## Execution Steps

1. **Check MCP** — Confirm the required Excalidraw MCP path in `references/mcp-delivery.md`. If missing, guide install and restart per **Tooling**.

2. **Read Input** — Ingest material; if only a URL, fetch body first.

3. **Choose mode & plan** — Pick diagram type using `references/diagram-modes.md` when needed; set title and layout; apply layout defaults from `references/layout-and-iteration.md` when vertical talking-head applies.

4. **Brief User** — Send visible briefing (including theme/title line), then draw.

5. **Load Format** — Call `read_me` (again on first draw of a new session if needed).

6. **Render** — Render the board via Excalidraw MCP.

7. **Browser fallback** — If the board is **not visible** in chat or the user wants the site, follow `references/mcp-delivery.md` and share the browser link.

8. **Polish** — Iterate; re-export with `export_to_excalidraw` when a share link is needed.

## Quick Checklist
- [ ] `read_me` / `create_view` (and `export_to_excalidraw` when needed) work; if not, user was guided and told to restart
- [ ] If no in-chat preview, excalidraw.com link was shared
- [ ] URL body fetched when input was link-only
- [ ] User received briefing; canvas has a **title/label** (any style)
- [ ] No script or recording-only text on the audience board
- [ ] For vertical talking-head, PiP margin considered when using portrait layout
- [ ] Anti-patterns reviewed if output quality is off (`references/pitfalls.md`)
