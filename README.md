# Proxylang for AI agents

Make a website readable in other languages from your AI coding tool. No
signup: your agent starts a free trial (2,000 words, 1 language), installs one
script tag, checks it works, and gives you one link to keep it.

## Agent Skill

Works with Claude Code, Codex, Cursor, GitHub Copilot, OpenCode and other
tools that read [Agent Skills](https://agentskills.io).

```bash
npx skills add proxylang/skills
```

Then ask your agent: "Make my website multilingual."

## MCP server

Remote, Streamable HTTP, no key needed:

```
https://proxylang.dev/mcp
```

Tools: `proxylang_start`, `proxylang_check`, `proxylang_next`,
`proxylang_status`.

Claude Code:

```bash
claude mcp add --transport http proxylang https://proxylang.dev/mcp
```

## Plain HTTP

Any agent can follow the guide directly: https://proxylang.dev/agents.md

## What it costs

The trial is free. Keeping the site translated after the trial, more
languages, image translation, search in any language and live chat are paid
plans: https://proxylang.dev/pricing
