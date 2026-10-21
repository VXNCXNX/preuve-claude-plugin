# Preuve AI for Claude Code

Claude Code plugin that connects Claude to [Preuve AI](https://preuve.ai) through its official remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Score a startup idea against live market evidence without leaving the terminal. Claude gets the same analysis as preuve.ai: a viability score, competitors, risks, and source-linked evidence.

## Install

```bash
claude plugin marketplace add VXNCXNX/preuve-claude-plugin
claude plugin install preuve@preuve
```

Or, inside a Claude Code session:

```text
/plugin install preuve --marketplace VXNCXNX/preuve-claude-plugin
```

Then start a new session, or run `/reload-plugins`.

## Sign in

Auth is OAuth. There is no API key or client ID to configure. Run `/mcp`, pick the Preuve server and sign in with Preuve in the browser it opens. The consent screen creates an API key for this connection.

## Skills

| Skill                    | What it does                                                                                                          |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| `/preuve:validate-idea`  | Runs a starter scan on your idea, waits for it to finish, and summarizes the verdict, score, competitors and top risk |
| `/preuve:generate-ideas` | Generates startup ideas from your interests and skills, then suggests which one to validate                           |

## Tools

| Tool              | What it does                                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `start_analysis`  | Validate a startup idea. A starter scan is the default. A deep scan runs only when you ask for one                                         |
| `get_analysis`    | Check whether a run has finished, with links to the report                                                                                 |
| `enrich_analysis` | Add sections to a deep report: founder fit, proof of demand, a launch playbook, trends, or community signals                               |
| `export_analysis` | Get a finished report as structured data: scores, verdict, risks, and competitors. Deep reports add market sizing, sections, and citations |
| `generate_ideas`  | Generate startup ideas from your interests                                                                                                 |
| `get_agency`      | Read a Consultant or Agency workspace: its project slots and client reports                                                                |

The hosted server is the source of truth for tool names and schemas.

## Notes

- Tool calls run as the Preuve account that approved the connection.
- A deep scan draws on that account's scan quota. Claude starts a starter scan unless you ask for a deep one.
- Each sign-in creates a key named `Claude Code (plugin:preuve:preuve) (OAuth)` in [Account → API Keys](https://preuve.ai/app?settings=apiKeys). Signing in again retires the previous key once the new one makes its first tool call. A previous key that was created in the last 15 minutes and never used stays active. Revoke it there if you no longer need it.

## Docs

- Preuve MCP server: https://preuve.ai/mcp
- Setup guide: https://docs.preuve.ai/mcp-server
- Privacy policy: https://preuve.ai/privacy
- Support: support@preuve.ai

## License

MIT
