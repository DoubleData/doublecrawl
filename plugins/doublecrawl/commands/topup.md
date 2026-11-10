---
description: Ask a person to approve more DoubleCrawl credits
argument-hint: <credits> [reason]
---

Arguments: $ARGUMENTS

The first argument is the number of credits (a positive whole number; accept `100000`, `100,000`
or `100k`). Everything after it is the reason. If the number is missing or invalid, ask for it and
stop. If there is no reason, write a one-sentence reason from the current task.

Call the `request_top_up` tool of the `doublecrawl` MCP server with `credits` and `reason`.

- `pending`: show the `approvalUrl` and the `code`, and say a person in the workspace must approve
  it in DoubleCrawl. Do not say the credits were bought or the payment happened.
- `charging` or `paid`: say it was approved automatically within the workspace policy and the
  credits arrive once the payment clears.
- Any other status or an error: show the `message` or error text as is.

Do not poll. Call `get_top_up_status` with the request `id` only when the user asks whether it was
approved.
