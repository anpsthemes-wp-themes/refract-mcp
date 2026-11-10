# Refract MCP server

[![mcptry](https://mcptry.com/badge/social.refract/refract)](https://mcptry.com/servers/social.refract/refract)

[Refract](https://refract.social) is a social media publishing app. Its remote MCP server lets AI apps such as Claude,
ChatGPT, Cursor and VS Code draft, approve and schedule posts for you.

```
https://mcp.refract.social
```

There is no token to paste. The app opens Refract, you sign in and pick the workspaces and what the app may do, and it
connects. Refract supports OAuth discovery, dynamic client registration and PKCE, so the app finds the sign-in on its
own.

- Transport: Streamable HTTP
- Auth: OAuth 2.1 (sign in with your Refract account)
- Overview and example prompts: [refract.social/mcp](https://refract.social/mcp)
- Full docs: [refract.social/docs/mcp](https://refract.social/docs/mcp)

## Connecting

### Claude

1. In Refract, open **Settings › API and agents**, select **Connect Claude**, pick the workspaces and what Claude can do,
   then select **Continue in Claude**.
2. In Claude, add a custom connector named Refract with the address above.
3. Select Connect, then sign in to Refract if it asks.
4. Check the workspaces and what Claude can do, then select Allow.
5. To let Claude upload files you attach in the chat, open **Settings › Capabilities** in Claude, turn on **Allow network
   egress** and add `mcp.refract.social` to **Additional allowed domains**.

### ChatGPT

1. In ChatGPT on the web, open Plugins, select +, then Add custom MCP server.
2. Name it Refract, paste the address and choose OAuth.
3. Accept the risk warning, select Create as a plugin, then sign in to Refract and pick what ChatGPT can do.

### Claude Code

Run this in your terminal, then type `/mcp` and sign in.

```bash
claude mcp add --transport http refract https://mcp.refract.social
```

### Cursor

Add this to `.cursor/mcp.json`, then sign in when Cursor asks.

```json
{ "mcpServers": { "refract": { "url": "https://mcp.refract.social" } } }
```

### VS Code

Add this to `.vscode/mcp.json`, then sign in when VS Code asks.

```json
{ "servers": { "refract": { "type": "http", "url": "https://mcp.refract.social" } } }
```

### Other apps

Add a remote MCP server (Streamable HTTP) with the address above. Refract signs you in through your browser with OAuth.

## What a connection can do

When you connect an app you pick its workspaces and one of two levels:

| Level                       | Works like    |                                                                                                               |
| --------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------- |
| Draft and send for approval | a Contributor | Reads, writes drafts, adds files and sends posts for approval                                                 |
| Schedule and publish        | an Editor     | All of that, schedules or publishes posts, sends failed posts again, and edits or removes posts that went out |

A connection never does more than your own role allows, and only sees the workspaces it was given. Tools it isn't
allowed are not offered to the app at all. Where a workspace asks for approval from AI agents, everything an app
schedules waits for approval, just like a teammate's post. Admins see every connection in **Settings › API and agents**
and can remove it at any time.

## Tools

| Tool                    | What it does                                                                                                              | Level            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| `list-workspaces`       | The workspaces this connection can reach, with their time zone and whether agent posts need approval                      | Any              |
| `list-accounts`         | A workspace's connected social accounts: id, platform, name, handle and health                                            | Any              |
| `list-posts`            | A workspace's posts, filtered by status and a date range, newest first (at most 50), with any waiting approval            | Any              |
| `get-post`              | One post: text, per-platform text, accounts, files, status, time, history, why an account failed and any waiting approval | Any              |
| `read-analytics`        | A workspace's numbers for a period: totals, each network, the best posts and what data is missing                         | Any              |
| `check-post`            | Runs the composer's checks on a saved post and lists its issues per network                                               | Any              |
| `find-slots`            | The next free posting times for some accounts, up to 10                                                                   | Any              |
| `create-draft`          | Creates a draft with text, accounts and files, and per network a different text and post fields                           | Draft or Publish |
| `update-draft`          | Changes a post that hasn't gone out; only what you pass changes, and a scheduled post keeps its time                      | Draft or Publish |
| `upload-media`          | Adds an image or video to the workspace's media library from a public https link or a file attached in ChatGPT            | Draft or Publish |
| `request-media-upload`  | A one-time link to upload an image or video from the app's computer or code sandbox, e.g. a file attached in chat         | Draft or Publish |
| `send-for-approval`     | Sends a draft for approval at a time, or at the next free posting time                                                    | Draft or Publish |
| `delete-draft`          | Deletes a draft, or a post waiting for approval with its request                                                          | Draft or Publish |
| `schedule-post`         | Schedules a draft at a time or the next free posting time; becomes an approval request where the workspace asks           | Publish          |
| `publish-now`           | Publishes a draft or a scheduled post now, on every account it goes to; it can't be undone                                | Publish          |
| `reschedule-post`       | Moves a scheduled post, or one waiting for approval, to a new time                                                        | Publish          |
| `unschedule-post`       | Takes a scheduled post, or one waiting for approval, back to the drafts                                                   | Publish          |
| `retry-post`            | Sends a failed post again to one account where it failed, fixing its files first when needed                              | Publish          |
| `edit-published-post`   | Edits a post that already went out, on one of its accounts, where the network allows edits                                | Publish          |
| `remove-published-post` | Removes a post that already went out from one of its accounts, where the network lets apps remove posts                   | Publish          |

Posts made through MCP show their source as `mcp` in the API and in Refract.

## About this repository

This repository holds the public listing metadata for Refract's hosted MCP server: `server.json` for the
[MCP Registry](https://registry.modelcontextprotocol.io) and `lhm.plugin.json` for the
[LobeHub Marketplace](https://lobehub.com/mcp). The server itself is part of the Refract app and is not open source.
