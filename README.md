# grabbit-skills

Agent skills for [Grabbit](https://www.grabbit.sh/) — the Reddit marketing and
lead-generation platform. Install them into Claude Code, Cursor, or any agent that
supports the [`skills`](https://www.skills.sh/) ecosystem.

## Skills

| Skill | What it does |
|---|---|
| [`grabbit`](skills/grabbit) | Drive the Grabbit MCP as a Reddit marketing agent: research subreddits and keywords, triage classified leads and brand mentions, and draft replies without getting banned. |

## Install

Install everything:

```bash
npx skills add grabbit-sh/grabbit-skills
```

Or a specific skill:

```bash
npx skills add grabbit-sh/grabbit-skills --skill grabbit
```

## Requirements

The `grabbit` skill drives the `mcp__grabbit__*` tools, so the [Grabbit MCP
server](https://www.grabbit.sh/) must be connected to your agent with a valid
account.

## License

MIT
