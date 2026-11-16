# pycon-se-26-mcp-demo

Demo setup for the talk "From API to Agent Tool: Connecting Your Apps with MCP" at PyCon Sweden 2026 ([slides](https://nezhar.com/slides/pycon-se-26/)).

A coding agent in a [VibePod](https://vibepod.dev/docs/) container connects to the VibePod board over MCP and signs in with OAuth. There is no token in this repository.

## Requirements

- **A running [vibepod-board](https://github.com/VibePod/vibepod-board).** The agent connects to its MCP endpoint at `http://localhost:3000/mcp`; without the board there is nothing to talk to. In a checkout of that repository run `docker compose up --build`, open `http://localhost:3000` and sign in as the admin (credentials: see its README). It has to be a version with OAuth sign-in, and it needs at least one project.
- **The [VibePod CLI](https://github.com/VibePod/vibepod-cli)** (`vp`, 0.21 or newer) and Docker. The board's compose file expects the Docker network `vibepod-network`: the first `vp run` creates it, or run `docker network create vibepod-network`.

## In this folder

| Path | What it is |
|---|---|
| `.pi/mcp.json` | MCP config for Pi's built-in MCP support: board URL and OAuth, no token |
| `.mcp.json` | The same server for Claude Code and other agents that read a project-level `.mcp.json`. It carries `"type": "http"`, which Claude Code requires. Pi does not read this file |
| `.vibepod/overlay/Dockerfile` | Adds two forwards to the agent container so OAuth sign-in works from inside it |

## Run it

In this folder:

```bash
vp run pi -p 127.0.0.1:8765:8766       # Pi, as in the talk
vp run claude -p 127.0.0.1:8765:8766   # or Claude Code
```

1. Trust the project when the agent asks.
2. Sign in to the board: `/mcp login vibepod-board` in Pi, `/mcp` in Claude Code. Open the sign-in address in the browser on the host, sign in as the board admin, tick the projects the agent may use and approve. The agent gets a token for exactly those projects.
3. Ask something, for example: `Tell me the number of tasks in the <project> project on the VibePod board`.

The published port is only needed for the sign-in. Why the two forwards are needed, and what to watch out for: [vibepod-cli PR #248](https://github.com/VibePod/vibepod-cli/pull/248).
