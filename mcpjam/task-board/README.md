# Task Board

A small ChatGPT App built on the Model Context Protocol (MCP). The server exposes task tools. A React widget renders the tasks inside the chat and lets you mark them done.

## Structure

```
.
├── server/          MCP server (Express + @modelcontextprotocol/sdk)
│   └── src/
│       ├── index.ts     HTTP entry point, serves /mcp on port 3000
│       └── server.ts    Tools and the UI resource
└── web/             Widget UI (React, bundled with esbuild)
    └── src/
        └── component.tsx
```

## Tools

| Tool              | Purpose                                                        |
| ----------------- | -------------------------------------------------------------- |
| `list_tasks`      | Returns tasks, optionally filtered by `status` (`open`/`done`). |
| `complete_task`   | Marks a task as done by `taskId`.                              |
| `show_task_board` | Renders the task board widget (`ui://task-board/v1.html`).     |

Tasks live in memory, so any change is lost when the server restarts.

## Getting started

Requires Node.js 18+.

1. Build the widget. The server reads `web/dist/component.js`, so do this first:

   ```bash
   cd web
   npm install
   npm run build
   ```

2. Start the server:

   ```bash
   cd server
   npm install
   npx tsx src/index.ts
   ```

   The MCP endpoint is at `http://localhost:3000/mcp`.

3. Connect it to ChatGPT or another MCP client. For ChatGPT, expose the local server with a public HTTPS tunnel such as ngrok, then add the tunnel URL plus `/mcp` as a connector.

Run `npm run build` in `web/` again after each UI change. The server reads the bundle on every request, so you don't need to restart it.
