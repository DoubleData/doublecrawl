---
name: doublecrawl
description: Use when a task needs the content of a web page (search results, listings, prices, any URL) that you cannot read directly, or when the user asks about DoubleCrawl credits, usage, request logs, invoices or a top-up. Fetches pages through the DoubleCrawl MCP server with the workspace's credits.
---

# DoubleCrawl

The `doublecrawl` MCP server fetches web pages through DoubleCrawl. Every fetch spends the
workspace's credits, within this agent's monthly budget.

## Fetching pages

Call `fetch_page` with `url` (http or https). Optional: `method` (`GET` or `POST`), `headers`
(not `Authorization` or `Host`), `country` (`us`, `de`, `gb`, `es`, `pl`, `jp`), `session_id` to
reuse one session across calls, and `format`.

- Use `format: "text"` when you only need to read the page: scripts, styles and tags are removed.
  Use the default `raw` when you need the HTML (links, attributes, structured data).
- Bodies are returned in pages of 50,000 characters. When `nextOffset` is not null, call again with
  the same arguments and `offset` set to `nextOffset`. Stop as soon as you have what you need: each
  call is a new request and costs credits.
- `outcome` says what happened at the target (`delivered` means the page came back); `status` is
  the target's HTTP status.

## Credits and budget

- `get_balance`: credits left in the workspace, per lot, with expiry dates.
- `get_usage`: requests, success rate, credits and top domains for `24h`, `7d` or `30d`.
- `list_requests`: recent requests, newest first (`q`, `outcome`, `limit`, `cursor`).
- `list_invoices`: invoices, only for an agent connected by an owner or admin.

## Top-ups: a person decides

When credits or the agent's budget run out, call `request_top_up` with `credits` and a short
`reason`. Normally it returns `pending` with an `approvalUrl` and a `code`: show both to the user
and tell them a person in the workspace must approve it. Never say credits were bought or a payment
was made while the status is `pending`. A workspace policy can approve small requests at once
(`charging`, then `paid`).

Check a request with `get_top_up_status` only when the user asks. Statuses: `pending`, `charging`,
`action_required` (the payment needs the person's confirmation), `paid`, `raised` (the agent's
budget was raised instead of buying credits), `declined`, `failed`, `expired` (not decided within
24 hours). An agent can ask for at most 5 top-ups in 24 hours.

## Errors

A failed call returns one line of text. Show it to the user as is.

- "Fetching is turned off…" or "Reading data is turned off…": the workspace's agent policy blocks
  this; a person can change it in DoubleCrawl.
- "This agent's monthly budget is used up…": ask a person to raise the budget, or request a top-up.
- "DoubleCrawl is unavailable, try again": a temporary failure; retry once later, do not loop.
- An authorization error from the MCP client: the agent was disconnected or its access expired;
  reconnect with `/mcp`.
