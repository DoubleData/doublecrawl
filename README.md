# DoubleCrawl for Claude Code

A Claude Code plugin marketplace with one plugin, `doublecrawl`: fetch web pages through
[DoubleCrawl](https://doublecrawl.com) from Claude Code with your workspace's credits.

## Install

```
/plugin marketplace add doubledata/doublecrawl
/plugin install doublecrawl@doublecrawl
```

Then run `/mcp`, pick `doublecrawl` and authenticate: DoubleCrawl opens in the browser, where you
choose the workspace and the agent's monthly budget.

## What it adds

- MCP server `doublecrawl` (`https://mcp.doublecrawl.com/mcp`) with `fetch_page`, `get_balance`,
  `get_usage`, `list_requests`, `list_invoices`, `request_top_up` and `get_top_up_status`.
- `/doublecrawl:balance`: credits left and the next expiring lot.
- `/doublecrawl:topup <credits> [reason]`: asks a person in the workspace to approve a top-up.
- Skill `doublecrawl`: when and how to fetch pages, budgets and top-ups, what errors mean.

## Local development

Point the plugin at another MCP server with `DOUBLECRAWL_MCP_URL`:

```
DOUBLECRAWL_MCP_URL=http://localhost:3100/mcp claude --plugin-dir plugins/doublecrawl
```
