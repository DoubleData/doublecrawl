---
description: Show the DoubleCrawl credits left in this workspace
---

Call the `get_balance` tool of the `doublecrawl` MCP server.

Report the total `credits` as a whole number with thousands separators. Then name the lot that
expires next (the lot with the earliest non-null `expiresAt`): its remaining credits and its expiry
date. If no lot expires, say so. Keep it to two short lines.

If the tool returns an error, show its message as is.
