# Demo runbook

PyCon Sweden 2026 · "From API to Agent Tool" · live part of the talk.

## In this folder

| Path | What it is |
|---|---|
| `.pi/mcp.json` | MCP config for Pi's built-in MCP support: board URL and OAuth, no token |
| `.mcp.json` | The same server for Claude Code and other agents that read a project-level `.mcp.json`. It carries `"type": "http"`, which Claude Code requires. Pi does not read this file |
| `.vibepod/overlay/Dockerfile` | Adds two forwards to the Pi container so OAuth sign-in works from inside it |

Pi runs in this folder: `vp run pi -p 127.0.0.1:8765:8766` or ``vp run claude -p 127.0.0.1:8765:8766``
