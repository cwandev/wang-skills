# MCP delivery

Use this file only when checking tool availability, rendering the board, or recovering from a missing inline preview.

## Required MCP path

Default delivery path:

1. Call `read_me` when starting fresh or when the element format is unclear.
2. Call `create_view` to render the board in chat.
3. If the board is not visible inline, call `export_to_excalidraw` and share the returned browser URL.

Do not replace this path with non-MCP drawing tools.

## Availability check

Before drawing:

- Confirm Excalidraw MCP is available in the current host.
- Confirm the session can access the tools needed for the default path above.
- If the host frequently fails to show inline boards, treat browser export as part of the expected delivery path.

## If MCP is unavailable

- Give the user copy-paste setup guidance based on [excalidraw-mcp](https://github.com/excalidraw/excalidraw-mcp).
- Explain that the host or agent session must be restarted after setup.
- Do not pretend the board was rendered if the MCP path is unavailable.

## If inline preview is missing

- Export the same board scene to a browser link.
- Share that link explicitly with the user.
- Do not stop after `create_view` if the result is invisible in chat.
